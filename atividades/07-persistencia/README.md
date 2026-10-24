# Atividade 07 — Persistência

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender, em laboratório controlado, como um atacante garante acesso contínuo e recorrente a um sistema já comprometido, e — do ponto de vista defensivo — como cada mecanismo de permanência é detectado e removido. A prática foi executada em ambiente isolado, sobre host autorizado, como etapa de pós-exploração.

> Esta página pública trata do conceito e da defesa. Os scripts e os comandos de persistência permanecem no repositório privado da disciplina; aqui não há receita pronta.

## Fundamento

A persistência é a etapa posterior ao acesso inicial: assegura o retorno ao sistema por meio de um mecanismo que sobreviva a uma reinicialização, à troca de senha ou à queda da sessão. O objetivo deixa de ser entrar e passa a ser não precisar entrar de novo, o que costuma exigir modificar algo que o sistema executa de forma automática e recorrente.

## O que a prática demonstrou

A partir do host comprometido, a pós-exploração estabeleceu um canal de retorno — uma conexão iniciada pela própria máquina-alvo de volta ao atacante, recebida por um ouvinte à espera. Esse canal foi então tornado persistente por dois mecanismos complementares do Linux:

- **Agendador de tarefas (cron):** uma tarefa programada para reexecutar o canal de retorno em intervalo fixo, garantindo a reconexão automática caso a sessão caísse.
- **Perfil de usuário (`.bashrc`):** uma instrução adicionada ao perfil, de modo que o canal fosse disparado silenciosamente a cada nova sessão de terminal.

O ponto a reter é que os dois mecanismos se apoiam em recursos legítimos do sistema — agendamento e perfil de shell — desviados de sua função. É justamente essa reutilização do legítimo que torna a persistência difícil de notar sem um inventário de referência.

## Defesa

A persistência tem uma característica a favor de quem defende: ela precisa permanecer, e o que permanece pode ser inventariado e comparado.

- **Inventariar o que deveria existir** — tarefas agendadas, serviços, contas — e investigar o que destoa. Uma tarefa de cron que reexecuta um script a cada minuto é um padrão anômalo evidente sob revisão.
- **Monitorar os pontos clássicos de persistência:** o cron de cada usuário, os arquivos de perfil (`.bashrc` e equivalentes) e os serviços de inicialização.
- **Registrar a autoria das alterações** nesses pontos, para que uma modificação fora do processo apareça.
- **Remover com base no inventário, não apenas o artefato visível:** uma persistência bem feita costuma ter mais de um ponto de retorno — neste laboratório, foram dois.

## Conclusão

A lição que incorporo é de detecção. Como a persistência precisa sobreviver no tempo e se esconde em recursos legítimos, ela é combatida menos por impedir e mais por perceber: um sistema que registra suas ações sensíveis e mantém um inventário de referência transforma a permanência do atacante em um desvio observável. É o mesmo princípio por trás da trilha de auditoria que mantenho no meu sistema, para que o anormal tenha onde aparecer.

## Evidências

A execução em ambiente controlado foi registrada por capturas de tela, preservadas como evidência no repositório privado da disciplina.
