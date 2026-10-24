# Atividade 01 — Reconhecimento e Footprinting

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Reunir o máximo de informação sobre um alvo autorizado antes de iniciar qualquer teste, distinguindo o que é possível obter sem interagir com o alvo (reconhecimento passivo) do que exige contato direto (reconhecimento ativo).

## Fundamento

O reconhecimento é a etapa inicial de um teste de intrusão e, ao mesmo tempo, a primeira oportunidade de defesa. Toda informação obtida nesta fase corresponde a uma superfície que a organização expôs. O reconhecimento passivo apoia-se em fontes abertas, como registros DNS, WHOIS, certificados, metadados de documentos e mecanismos de busca, e praticamente não deixa rastros. O ativo consulta o alvo de forma direta e, por esse motivo, é passível de detecção.

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

A principal lição que extraio é que defender começa por reduzir aquilo que se deixa visível: retirar do ar serviços que não são utilizados, suprimir banners que revelam versões e limpar os metadados dos arquivos publicados. No sistema que desenvolvo, apoiado em Firebase e Cloud Functions, a superfície pública se resume à página do cliente e aos endpoints. Manter as chaves de API e as regras de acesso fora do alcance do cliente é a aplicação direta do que estudei nesta atividade.

## Evidências

As imagens a seguir documentam a execução. O hostname, o endereço IP público e eventuais tokens foram tarjados.

![descrição da evidência](./imagens/exemplo.png)
