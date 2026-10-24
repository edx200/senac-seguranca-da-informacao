# Atividade 07 — Persistência

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender como um atacante procura manter o acesso após o ingresso inicial e, do ponto de vista defensivo, como essa permanência é detectada e removida.

## Fundamento

A persistência consiste em assegurar o retorno ao sistema por meio de um mecanismo que sobreviva a uma reinicialização ou à troca de senha, como uma tarefa agendada, um serviço instalado ou uma conta criada. É uma etapa posterior ao acesso inicial.

## Execução

Descrevo nesta seção a sequência de decisões da execução que realizei. Os comandos reproduzidos abaixo restringem-se aos que atendem ao critério definido no [README](../../README.md): aqueles que auxiliam a compreender ou a defender e que não constituem uma receita pronta. Credenciais, payloads e a cadeia completa de exploração permanecem no repositório privado da disciplina.

```bash
# comando executado
```

Saída obtida:

```
# saída real, com os dados sensíveis substituídos por <REDIGIDO>
```

A leitura dessa saída indica [interpretação do resultado].

## Dificuldades

[o erro encontrado, a mensagem exata e a forma como foi contornado]

## Conclusão

Observo que toda persistência deixa vestígios, e vestígios são detectáveis: uma tarefa agendada que ninguém criou, uma conta surgida fora do processo, um serviço desconhecido. A defesa está em manter o inventário do que deveria existir, monitorar as alterações naquilo que persiste entre reinicializações e registrar a autoria de cada criação. A lição que incorporo é a de detecção: um sistema precisa registrar as ações sensíveis, como a criação de um usuário, para que o comportamento anômalo se torne visível, exatamente como fiz ao implementar a trilha de auditoria. O procedimento ofensivo permanece no repositório privado.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
