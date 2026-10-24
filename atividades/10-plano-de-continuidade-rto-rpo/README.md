# Atividade 10 — Plano de Continuidade: RTO, RPO e Atividades Críticas

**Disciplina:** Resiliência Cibernética · **Atividade em grupo:** Eduardo Pereira Correa e Julia Mariano

---

## Objetivo

Conduzir a análise de impacto no negócio (BIA) de uma organização fictícia — a Cerrado Store, um e-commerce de artigos esportivos — para identificar suas atividades críticas e definir, com justificativa quantitativa, o RTO e o RPO de cada uma. O exercício não é escolher números: é demonstrar, a partir das evidências do cenário, por que cada número é aquele e não outro.

## Fundamento

Três métricas estruturam um plano de continuidade:

- **MTPD** (Maximum Tolerable Period of Disruption) — o tempo máximo que uma atividade pode ficar parada antes de causar dano irreversível ao negócio.
- **RTO** (Recovery Time Objective) — a meta de tempo para restaurar a atividade. Precisa ser sempre menor que o MTPD.
- **RPO** (Recovery Point Objective) — o máximo de dados que se aceita perder, medido em tempo: um RPO de 15 minutos exige cópias no máximo a cada 15 minutos.

A regra de coerência que amarra tudo: para toda atividade, **RTO < MTPD**, e o RPO definido tem de ser tecnicamente sustentado pela rotina de backup real — não pela desejada.

## A análise

Classifiquei as atividades da organização entre críticas e não críticas a partir dos produtos fundamentais que cada uma sustenta. Quatro ficaram como críticas: integração de pedidos (middleware), controle de estoque, separação/expedição e emissão de NF-e. As não críticas (pós-venda, precificação, compras, marketing) foram justificadas por tolerarem interrupção de 48 a 72 horas sem dano irreversível.

O núcleo do raciocínio é um cálculo de folga contratual. Os marketplaces suspendem o vendedor acima de 5% de atraso em 30 dias, e o índice já estava em 3,1%. Sobre 144.000 pedidos no período, a folga de 1,9 ponto percentual equivale a 2.736 pedidos — e, ao ritmo declarado de 530 pedidos separados por hora, a apenas **5,16 horas de parada acumulada no mês inteiro**. Como a janela de expedição (9 h × 530 = 4.770/dia) já roda abaixo da demanda média (4.800/dia), não há capacidade ociosa: cada hora parada é definitivamente perdida. É esse agravante que fixa o MTPD das atividades da janela em 5 horas.

A partir daí, cada RTO e RPO foi ancorado em evidência. A emissão de NF-e, por exemplo, recebeu RTO menor (1 hora) que a separação (2 horas), porque bloqueia o trabalho já concluído: uma parada às 15h retém também todos os volumes já separados e ainda não etiquetados. O RPO de 15 minutos foi justificado pelo que é reconstituível por trabalho manual — cerca de 132 registros, reconferíveis em menos de uma hora — contra as 4.800 movimentações de um dia inteiro, que exigiriam inventário físico com a operação parada.

## O achado principal

A maior lacuna da organização não é de tecnologia, é de verificação: nenhum teste de restauração havia sido feito sobre um banco de 800 GB, com backup diário para um NAS no mesmo rack do servidor e cópia externa esporádica. A conclusão que registrei resume o aprendizado da atividade:

> A organização não tem um RTO ruim — não tem RTO nenhum, porque um objetivo de recuperação sem teste cronometrado é uma intenção, não uma métrica.

O backup no mesmo rack agrava o quadro: em incêndio, furto, alagamento ou ransomware, dado e cópia se perdem no mesmo evento.

## Recomendações, em ordem de prioridade

1. **Gerador com nobreak dimensionado** — menor custo relativo, ataca a causa mais frequente de indisponibilidade (três quedas de energia por mês, de até 4 horas, contra 15 minutos de autonomia) e elimina o risco de corrupção do banco por desligamento abrupto.
2. **Teste de restauração cronometrado** — transforma a intenção de recuperação em métrica real.
3. **Envio de log de transação a cada 15 minutos com cópia off-site imutável** — torna o RPO de 15 minutos sustentável (o volume é trivial: ~16 MB por ciclo no link de 300 Mbps).
4. **Monitoramento com alerta ativo** do middleware, cuja falha é silenciosa.

## Conclusão

A atividade mostrou que continuidade se decide no cálculo, não no palpite: o RTO de cada processo saiu de uma conta de folga contratual, e o RPO, do que é fisicamente reconstituível. É um raciocínio que aplico ao meu próprio sistema, que guarda dados de clientes: definir quanto tempo posso ficar fora e quanto dado posso perder é o que separa um backup que existe de um backup em que se pode confiar. O elo mais fraco quase nunca é a ferramenta — é a restauração que ninguém testou.
