# Atividade 07 — Persistência

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender, em laboratório controlado, como um atacante procura manter o acesso a um sistema após o ingresso inicial e, do ponto de vista defensivo, como essa permanência é detectada e removida. A prática foi executada em ambiente isolado, sobre alvo autorizado.

> Esta página pública trata do conceito e da defesa. A execução foi documentada por capturas de tela, mantidas como evidência no repositório privado da disciplina; o procedimento ofensivo não é reproduzido aqui.

## Fundamento

A persistência é a etapa posterior ao acesso inicial: consiste em assegurar o retorno ao sistema por meio de um mecanismo que sobreviva a uma reinicialização ou à troca de senha. Esse mecanismo costuma assumir a forma de uma tarefa agendada, um serviço instalado, uma chave de acesso adicionada ou uma conta criada para esse fim.

O objetivo do atacante, nesta fase, deixa de ser entrar e passa a ser não precisar entrar de novo. Por isso a persistência quase sempre modifica algo que o sistema executa de forma automática e recorrente — exatamente o tipo de alteração que um inventário bem mantido torna visível.

## Defesa

A persistência tem uma característica que joga a favor de quem defende: ela deixa rastro. Diferente de uma ação pontual, precisa permanecer, e o que permanece pode ser inventariado e comparado.

- **Manter o inventário do que deveria existir** — tarefas agendadas, serviços, contas e chaves de acesso legítimas — para que o que é novo se destaque.
- **Monitorar as alterações no que persiste entre reinicializações**, ponto em que os mecanismos de persistência se revelam.
- **Registrar a autoria de cada criação** de conta, serviço ou tarefa, de modo que uma origem anômala apareça.
- **Remover com base no inventário**, e não só o artefato visível: uma persistência bem feita costuma ter mais de um ponto de retorno.

## Conclusão

A lição que incorporo é de detecção. Como a persistência precisa sobreviver no tempo, ela é combatida menos por impedir e mais por perceber: um sistema que registra suas ações sensíveis — a criação de um usuário, de um serviço, de uma tarefa — transforma a permanência do atacante em um evento observável. É o mesmo princípio que apliquei ao manter uma trilha de auditoria no meu sistema, para que o anormal tenha onde aparecer.

## Evidências

A execução em ambiente controlado foi registrada por capturas de tela, preservadas como evidência no repositório privado da disciplina.
