# Atividade 03 — Análise de Tráfego com Wireshark

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Capturar e interpretar o tráfego de rede em ambiente de laboratório para observar o que cada protocolo efetivamente transmite e por que o tráfego em texto claro representa um risco.

## Fundamento

O Wireshark apresenta o pacote exatamente como ele trafega. Em protocolos sem cifragem, como HTTP, FTP e Telnet, o usuário e a senha circulam de forma legível para qualquer um posicionado no caminho da comunicação. Sob TLS, o mesmo conteúdo torna-se ininteligível.

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

Esta prática é, para mim, o argumento mais convincente em favor do uso de TLS em todas as comunicações: observar uma credencial surgir em claro em um filtro de captura é mais persuasivo do que qualquer recomendação normativa. Reforça a decisão que mantenho no meu sistema, no qual nenhuma informação sensível, como o PIN ou as chaves, trafega em claro ou é validada no cliente.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
