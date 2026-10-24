# Atividade 11 — Alta Disponibilidade de Banco de Dados

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Implantar um ambiente de alta disponibilidade para banco de dados, de modo que a falha de um servidor não derrube o serviço. A arquitetura montada foi um cluster **MariaDB Galera** de múltiplos nós, com um **HAProxy** à frente distribuindo as conexões e isolando o nó que falhar.

> A execução em laboratório foi registrada por captura de tela, preservada como evidência no repositório privado da disciplina.

## Fundamento

Backup responde à pergunta "como volto depois que perdi"; alta disponibilidade responde a outra, anterior: "como evito parar". São camadas complementares da resiliência. A disponibilidade se constrói eliminando o **ponto único de falha** — o componente cuja queda, sozinha, derruba todo o serviço. Em um banco de dados tradicional, o próprio servidor é esse ponto.

Dois mecanismos atacam isso:

- **Replicação síncrona (Galera).** Em vez de um servidor único, o dado é escrito em vários nós ao mesmo tempo. Na replicação síncrona, uma transação só é confirmada quando o conjunto de nós a aceitou, de modo que todos têm a mesma informação e qualquer um pode assumir sem perda — diferente da replicação assíncrona, em que um nó secundário pode estar atrasado no momento da falha.
- **Balanceador com verificação de saúde (HAProxy).** As aplicações não se conectam a um nó específico, mas ao HAProxy, que encaminha para um nó vivo. Quando um nó deixa de responder à checagem de saúde, o HAProxy para de enviar tráfego para ele e o serviço continua pelos demais — sem a aplicação precisar saber que algo caiu.

## O que a prática demonstrou

A prática montou o cluster, pôs o HAProxy à frente e exercitou o **failover**: com o ambiente no ar, a interrupção de um nó deve ser absorvida pelos restantes, mantendo o banco acessível pela mesma porta, através do balanceador. É a diferença entre uma falha de servidor ser uma indisponibilidade e ser um não-evento para quem usa o sistema.

## Leitura defensiva

Alta disponibilidade é resiliência contra falha, não contra perda — e os dois pontos merecem registro:

- **HA não substitui backup.** O cluster protege contra a queda de um nó, mas replica fielmente também um `DROP TABLE` ou uma corrupção lógica para todos os nós. Disponibilidade e recuperação resolvem problemas diferentes; um ambiente sério tem os dois.
- **O balanceador não pode virar o novo ponto único de falha.** Um HAProxy sozinho à frente de um cluster redundante apenas desloca o ponto único para ele. Em produção, o próprio balanceador precisa ser redundante (um segundo HAProxy com IP virtual em failover).
- **O failover só existe se for testado.** Um cluster que nunca teve um nó derrubado de propósito é uma suposição de disponibilidade, do mesmo modo que um backup nunca restaurado é uma suposição de recuperação.

## Conclusão

A atividade fecha, com a camada de disponibilidade, o par que sustenta a resiliência de um sistema: evitar a parada (alta disponibilidade) e voltar da perda (backup). Para o sistema que mantenho, cujos restaurantes dependem dele em horário de operação, pensar a disponibilidade como ausência de ponto único de falha — e não como um servidor que "costuma funcionar" — é o que separa um sistema que uma equipe confia de um que ela teme.

## Evidências

A montagem e o teste em laboratório foram registrados por captura de tela, preservada no repositório privado da disciplina.
