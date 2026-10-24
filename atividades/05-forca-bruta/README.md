# Atividade 05 — Força Bruta

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender, na prática, por que um mecanismo de autenticação sem limite de tentativas é vulnerável à força bruta, e verificar quais defesas tornam o ataque inviável.

## Fundamento

Um ataque de força bruta consiste em submeter senhas sucessivas a um ponto de autenticação até encontrar a correta. O fator decisivo não está no atacante, e sim na defesa: o comprimento e a aleatoriedade da senha e, sobretudo, se o sistema permite tentativas ilimitadas.

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

Concluo que a contramedida mais eficaz não é exigir uma senha complexa, mas limitar a tentativa. Essa é precisamente a decisão que adotei no sistema que mantenho em produção: o PIN não é validado no cliente. Uma função no servidor compara o hash SHA-256, bloqueia o acesso por quinze minutos após cinco tentativas e registra a ocorrência em auditoria. Esta atividade formaliza o raciocínio por trás daquela escolha.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
