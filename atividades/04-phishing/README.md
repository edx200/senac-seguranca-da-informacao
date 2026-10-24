# Atividade 04 — Phishing

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender como um ataque de phishing é estruturado e, sobretudo, de que forma ele é identificado e contido pela perspectiva de quem defende.

## Fundamento

O phishing explora a pessoa, e não o sistema. Consiste em uma mensagem que simula uma fonte confiável para induzir um clique, a entrega de uma credencial ou a abertura de um anexo. É o vetor de acesso inicial mais frequente justamente por contornar as proteções técnicas.

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

Concentro-me nos indícios que denunciam a fraude: um remetente que não corresponde ao domínio legítimo, a urgência artificial, um link cujo destino difere do texto exibido e a solicitação de credenciais fora do fluxo habitual. A defesa é feita em camadas: autenticação de e-mail por SPF, DKIM e DMARC, um segundo fator que resista ao roubo de senha e a capacitação de quem recebe as mensagens. O procedimento ofensivo executado no laboratório está registrado no repositório privado da disciplina; nesta versão pública restrinjo-me ao reconhecimento e à defesa.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
