# Atividade 09 — Backup e Restauração

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Montar uma rotina de backup cifrado e fora da máquina de origem, e provar que ela funciona pelo que realmente importa: a restauração. O foco não foi só copiar dados, mas recuperar um arquivo específico sem destruir o resto e garantir que o backup aconteça sozinho, mesmo após uma reinicialização. Ambiente: servidor Ubuntu em AWS EC2.

## Fundamento

Backup é a última linha da resiliência: quando a prevenção falha — transação apagada por engano, ransomware, falha de disco — é o que permite voltar. Mas um backup só tem valor se a restauração for testada; backup que nunca foi restaurado é uma suposição, não uma garantia. Dois princípios guiaram a prática: o repositório é **cifrado** (quem acessar a nuvem não lê os dados) e fica **fora da origem** (uma falha no servidor não leva junto a cópia).

A ferramenta foi o **restic** (repositório criptografado, deduplicado), com o **rclone** levando os dados ao Google Drive. A política de retenção: 7 diários, 4 semanais e 6 mensais, aplicada com `restic forget --prune`.

## Desafio 1 — Restauração seletiva de um arquivo

O objetivo foi recuperar **apenas** um arquivo apagado, sem usar o `restore.sh`, que sobrescreveria todo o diretório de dados. A estratégia foi restaurar isolado em um diretório de preparação, conferir e só então devolver ao original — evitando tocar em qualquer outro arquivo de produção.

```bash
# localizar o arquivo nos snapshots
export RESTIC_REPOSITORY="rclone:drive:backup/restic-lab"
export RESTIC_PASSWORD_FILE="/home/ubuntu/.restic-password"
restic snapshots
restic find projetoA.txt

# restaurar somente o arquivo, para um diretório de preparação
restic restore latest --target /tmp/restauracao_projeto \
  --include /home/ubuntu/dados/projetos/projetoA.txt
```

O filtro `--include` limitou a restauração a um único arquivo. Depois de conferir dono, permissões, tamanho e conteúdo, copiei o arquivo de volta ao local original — nenhum outro arquivo do diretório foi alterado.

## Desafio 2 — Agendamento com cron

O `backup.sh` já exportava as variáveis e usava caminhos absolutos, então rodava sem depender do ambiente do terminal. Configurei o cron para rodar o backup e sobreviver a reinicializações:

```bash
# frequência definitiva pela política (diária), com a limpeza de retenção logo depois
0 2 * * * /home/ubuntu/backup.sh >> /home/ubuntu/backup_cron.log 2>&1
30 2 * * * ... restic forget --keep-daily 7 --keep-weekly 4 --keep-monthly 6 --prune >> /home/ubuntu/backup_cron.log 2>&1

# sobreviver a reinicializações
@reboot sleep 60 && /home/ubuntu/backup.sh >> /home/ubuntu/backup_cron.log 2>&1
```

A saída de cada execução é gravada em log para auditoria, e `systemctl is-enabled cron` confirmou que o serviço sobrevive ao boot. O horário `02:00 UTC` corresponde a 23:00 em Brasília.

## Dificuldades

A primeira tentativa de restauração falhou com `context deadline exceeded`, e a API do Google Drive retornou `403 rateLimitExceeded`. A causa é o limite de requisições do `client_id` padrão do rclone, que é **compartilhado** entre todos os usuários da ferramenta. A operação concluiu ao repetir após alguns segundos. A correção adequada para produção é criar um `client_id` próprio no Google Cloud e informá-lo ao rclone, deixando de disputar a cota com o resto do mundo.

## Conclusão

A atividade reforçou que a parte difícil do backup não é guardar, é restaurar — e restaurar com cirurgia, recuperando o que se perdeu sem sobrescrever o que está íntegro. Cifrar o repositório e mantê-lo fora da origem transforma a cópia em um ativo de resiliência de verdade, e não em mais um ponto de falha. É o tipo de garantia que o sistema que mantenho precisa ter: os dados dos clientes não podem depender de um único disco nem de uma única máquina.

## Evidências

As capturas das execuções (snapshots, restauração seletiva, cron e validação pós-reboot) acompanham o relatório completo entregue na disciplina.
