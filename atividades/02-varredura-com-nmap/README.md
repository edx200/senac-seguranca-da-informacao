# Atividade 02 — Varredura com Nmap

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Mapear as portas abertas, os serviços e as versões em execução em um host autorizado, compreendendo o que cada modalidade de varredura revela e o volume de ruído que produz.

## Fundamento

O Nmap responde a três perguntas: quais portas estão abertas, qual serviço responde em cada uma e qual a sua versão. A versão é a informação mais sensível, pois é ela que associa um serviço a uma vulnerabilidade conhecida, catalogada como CVE.

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

Entendo o resultado de uma varredura como a relação de tudo o que preciso justificar: uma porta aberta sem finalidade é um risco desnecessário. A defesa consiste em encerrar o que não é usado, manter atualizado o que permanece e tratar a própria varredura como um evento detectável. É o mesmo exame que faço ao revisar aquilo que o meu backend expõe.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
