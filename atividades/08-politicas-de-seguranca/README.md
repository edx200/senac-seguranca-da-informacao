# Atividade 08 — Políticas de Segurança

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Converter a prática técnica em norma organizacional, definindo o que é permitido, a quem é permitido e o que deve ser registrado.

## Fundamento

A política de segurança é a camada que não se expressa em código: estabelece papéis e níveis de acesso, o tratamento dos dados pessoais, a resposta a incidentes e as regras que o técnico posteriormente implementa. Sem ela, cada controle técnico passa a depender de uma decisão isolada.

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

Entendo que política e técnica se sustentam mutuamente. Uma regra de menor privilégio só tem valor se o sistema a impõe, e um controle técnico sem política documentada não resiste à troca das pessoas envolvidas. No Brasil, a LGPD transforma parte dessas exigências em obrigação legal para quem trata dados de clientes. No meu sistema, que processa dados de clientes de restaurantes, isso se traduz no isolamento por tenant definido nas regras de acesso, na coleta acompanhada de aviso de LGPD no ponto de entrada e no controle de acesso por papel. Esta atividade constitui a moldura formal daquilo que já aplico.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
