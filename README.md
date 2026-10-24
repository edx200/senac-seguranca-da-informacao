# Segurança da Informação — Senac-DF

Atividades práticas e analíticas do curso de **Segurança da Informação**, 4º semestre, Faculdade de Tecnologia e Inovação Senac-DF. A maior parte são laboratórios da disciplina de **Resiliência Cibernética**; cada atividade indica a sua disciplina no cabeçalho.

Cada atividade aqui traz **o raciocínio técnico, os comandos seguros de publicar e o que eu aprendi**. As entregas avaliadas e a execução completa ficam em repositório privado com o professor.

---

## O que entra aqui e o que não entra

Nem todo comando de um laboratório de segurança pode ser publicado. Uso este critério, atividade por atividade:

| ✅ Publico | 🔒 Fica no privado |
|---|---|
| Enumeração e análise em alvo próprio (`nmap -sV`, filtros do Wireshark) | Cadeia de exploração que funciona de ponta a ponta |
| Comandos de **defesa** e endurecimento (regras de firewall, SSH, fail2ban) | Payload pronto para copiar e colar contra terceiro |
| Leitura e interpretação de saída | Credencial, hash, chave ou token — **mesmo de laboratório** |
| IP privado de lab (`10.x`, `172.16.x`, `192.168.x`) | IP público, hostname real ou nome de máquina da instituição |
| Ferramenta e versão usadas | Vulnerabilidade ainda não corrigida em sistema de terceiro |

**A regra que resume:** publico o que ensina a **entender e defender**; guardo o que serve de **receita pronta para atacar**.

Isso vale também para os colegas — atividade com o passo a passo completo exposto vira gabarito, e aprender a fazer é o ponto da disciplina.

---

## Atividades

| # | Atividade |
|---|---|
| 01 | [Reconhecimento e Footprinting](atividades/01-reconhecimento-e-footprinting/) |
| 02 | [Varredura com Nmap](atividades/02-varredura-com-nmap/) |
| 03 | [Análise de Tráfego com Wireshark](atividades/03-analise-de-trafego-wireshark/) |
| 04 | [Phishing](atividades/04-phishing/) |
| 05 | [Força Bruta](atividades/05-forca-bruta/) |
| 06 | [Acesso Inicial com Exploit](atividades/06-acesso-inicial-com-exploit/) |
| 07 | [Persistência](atividades/07-persistencia/) |
| 08 | [Políticas de Segurança](atividades/08-politicas-de-seguranca/) |

---

## Como ler cada atividade

Cada pasta em [`atividades/`](atividades/) segue a mesma estrutura:

1. **Objetivo** — o que a atividade pedia
2. **Ambiente** — máquinas, sistemas e ferramentas
3. **O que eu fiz** — a sequência de decisões, em texto corrido
4. **Comandos** — os que passam no critério acima, com a saída e a leitura dela
5. **O que deu errado** — e como contornei
6. **O que eu levo disso** — a conclusão prática
7. **Evidências** — prints, com dado sensível tarjado

---

## Ambiente de laboratório

| Item | Uso |
|---|---|
| Kali Linux | SSD externo com LUKS, para as práticas ofensivas |
| Windows 11 | estação principal, disco com VeraCrypt |

Todas as práticas acontecem em **laboratório isolado ou em alvo próprio, com autorização**. Nada aqui foi executado contra sistema de terceiro.

---

## Contexto

Desenvolvedor do [Comanda Vexto](https://github.com/edx200/comandas-pro-portfolio), SaaS em produção com restaurantes usando diariamente — onde tomo decisões de segurança que valem dinheiro de cliente. Esta disciplina é a ponte entre escrever o sistema e saber atacá-lo. [Perfil completo](https://github.com/edx200).
