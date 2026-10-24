# Atividade 08 — Políticas de Segurança

**Disciplina:** Gestão da Segurança da Informação

> Esta atividade pertence à disciplina de Gestão da Segurança da Informação. Foi incluída aqui por complementar o conjunto prático de Resiliência Cibernética com a camada de governança.

---

## Objetivo

Analisar a estrutura de uma Política de Segurança da Informação, identificando os elementos que a tornam aplicável: o que ela protege, a quem se aplica, quais são as responsabilidades e quais regras devem ser seguidas. A análise tomou como referência a política da empresa fictícia TechSafe Ltda., apresentada em aula.

## Fundamento

A política de segurança é a camada que não se expressa em código: estabelece papéis e níveis de acesso, o tratamento dos dados e as regras que o técnico posteriormente implementa. Sem ela, cada controle técnico passa a depender de uma decisão isolada. Analisei a política pelos seus quatro elementos estruturais.

**Objetivo.** Proteger os ativos de informação da organização, estabelecendo diretrizes para preservar a segurança das informações e orientar seu uso adequado.

**Escopo.** Define a quem e a quais recursos a política se aplica. No exemplo analisado, alcança todos os colaboradores, terceiros e sistemas, e não apenas a equipe de TI — o que é coerente, já que o uso dos ativos não se restringe à área técnica.

**Responsabilidades.** Estabelecem os papéis de cada parte: alta direção, gestores e usuários têm atribuições definidas. Isso determina quem cumpre as regras e quem responde pela gestão, supervisão e proteção dos ativos.

**Diretrizes.** São as regras a seguir. No exemplo, constam controle de acesso, uso aceitável e gestão de incidentes, que orientam o comportamento dos usuários e reduzem riscos no uso dos recursos.

## Aplicação ao meu contexto

Mantenho em produção um sistema que processa dados de clientes de restaurantes, e os quatro elementos analisados têm correspondência direta na sua operação. O escopo e as responsabilidades aparecem no controle de acesso por papel (administrador, gerente, garçom e cozinha), em que cada função alcança apenas o que lhe cabe. As diretrizes de controle de acesso se concretizam no isolamento por tenant definido nas regras do banco, que impede um restaurante de alcançar os dados de outro. E o tratamento de dado pessoal, exigido pela LGPD para quem lida com informação de cliente, está materializado no aviso de coleta no ponto de entrada. Esta atividade é a moldura formal daquilo que já aplico na prática.

## Conclusão

Uma Política de Segurança da Informação precisa apresentar com clareza o que pretende proteger, a quem se aplica, quais são as responsabilidades e quais regras devem ser seguidas. Política e técnica se sustentam mutuamente: uma regra de menor privilégio só tem valor se o sistema a impõe, e um controle técnico sem política documentada não resiste à troca das pessoas envolvidas.

---

**Referência:** SENAC-DF. Gestão da Segurança da Informação — Aula 05: Políticas de Segurança da Informação. Prof. Dr. Ronaldo Bach, 2026.2.
