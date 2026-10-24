# Atividade 05 — Força Bruta

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Simular ataques de força bruta online e offline contra um alvo vulnerável em ambiente isolado e, a partir das fragilidades identificadas, aplicar contramedidas de endurecimento (hardening). O exercício percorre o ciclo completo: demonstrar o ataque e, em seguida, neutralizá-lo. Ambiente montado em VirtualBox com rede Host-Only: Kali Linux como atacante e Metasploitable 2 como alvo.

> Esta página pública enfatiza o resultado e a defesa. A sintaxe operacional das ferramentas de ataque permanece no repositório privado da disciplina.

## Fundamento

Um ataque de força bruta submete senhas sucessivas a um ponto de autenticação até encontrar a correta. O que decide o resultado não é o atacante, e sim a defesa: o comprimento e a aleatoriedade da senha e, sobretudo, se o sistema permite tentativas ilimitadas. A força bruta pode ser **online** (contra um serviço ativo, como SSH ou um formulário web) ou **offline** (contra hashes já exfiltrados, sem tocar no alvo).

## O que a prática demonstrou

**Reconhecimento.** Com `netdiscover` descobri o alvo na sub-rede sem conhecer o IP de antemão, e com `nmap -sV` levantei os serviços: OpenSSH 4.7p1 na porta 22 e Apache 2.2.8 na 80.

**Ataques online.** Usei o Hydra contra o serviço SSH e, com o módulo `http-get-form`, contra o formulário de login da aplicação DVWA. Ambos os serviços, sem limite de tentativas, cederam a um dicionário.

**Ataque offline.** Simulando o cenário posterior a um comprometimento, extraí `/etc/passwd` e `/etc/shadow`, combinei-os com `unshadow` e submeti os hashes ao John the Ripper com o dicionário RockYou. As senhas fracas foram recuperadas.

A leitura é direta: enquanto o serviço aceita tentativas ilimitadas e as senhas são fracas, o ataque é só questão de tempo.

## Defesa e hardening

Apliquei o endurecimento no `sshd_config` do alvo e remedi o efeito:

| Parâmetro | Efeito |
|---|---|
| `Port 2222` | tira o serviço da porta padrão; anula varreduras automáticas que só olham a 22 |
| `PermitRootLogin no` | bloqueia o login direto do superusuário |
| `MaxAuthTries 3` | limita as tentativas por conexão, quebrando a viabilidade do dicionário |

Após reiniciar o serviço, a re-execução do Nmap confirmou que o ataque pela porta 22 foi bloqueado.

| Vetor | Estado inicial | Pós-hardening |
|---|---|---|
| Força bruta SSH | vulnerável (porta 22) | mitigado (porta 2222 + MaxAuthTries) |
| Acesso root direto | permitido | bloqueado (`PermitRootLogin no`) |
| Hashes fracos | quebrados pelo John | exige política de senha forte |

Recomendações finais: autenticação por chave pública SSH, controle de taxa de tentativas (como Fail2ban) e política severa de complexidade de senha.

## Conclusão

A contramedida mais eficaz não é exigir uma senha complexa, mas limitar a tentativa: foi o `MaxAuthTries` que tirou a viabilidade do ataque online, independentemente da força da senha. É precisamente a decisão que adotei no sistema que mantenho em produção, no qual o PIN não é validado no cliente — uma função no servidor compara o hash, bloqueia o acesso por quinze minutos após cinco tentativas e registra a ocorrência em auditoria. Esta atividade é a demonstração prática do raciocínio por trás daquela escolha.

## Evidências

As capturas da execução e do resultado pós-hardening acompanham o relatório completo entregue na disciplina.
