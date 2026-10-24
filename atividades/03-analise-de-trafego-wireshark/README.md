# Atividade 03 — Análise de Tráfego com Wireshark

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Capturar e interpretar o tráfego de rede para observar o que cada protocolo efetivamente transmite, do nome que o DNS resolve à abertura da conexão TCP, e demonstrar na prática por que um formulário enviado sobre HTTP expõe os dados de quem o preenche. A captura foi feita contra sites públicos de teste (`httpforever.com` e `httpbin.org`), em rede local.

## Fundamento

O Wireshark apresenta o pacote exatamente como ele trafega. Dominar seus filtros de exibição é o que transforma uma captura bruta em análise: cada filtro isola uma camada do que aconteceu na rede. A leitura que importa, do ponto de vista de segurança, é que a cifragem não é um detalhe — é a diferença entre um pacote legível e um pacote inútil para quem o intercepta.

## Execução

**Preparação.** Limpei o cache de DNS para forçar consultas reais durante a captura:

```
ipconfig /flushdns
```

**Resolução de nomes (filtro `dns`).** Observei a consulta de tipo A saindo do cliente (`192.168.1.105`) para o gateway (`192.168.1.1`) e a resposta: `httpforever.com` resolveu para `146.190.62.219`. A consulta e a resposta trafegam abertas.

**Abertura da conexão (filtro `ip.addr == 146.190.62.219 && tcp`).** Identifiquei o *three-way handshake* completo, a negociação que abre toda conexão TCP:

| Passo | Flags | Sentido | Observado |
|---|---|---|---|
| 1 | `SYN` | cliente → servidor | Seq=0, porta destino 80 |
| 2 | `SYN, ACK` | servidor → cliente | Seq=0, Ack=1 |
| 3 | `ACK` | cliente → servidor | Seq=1, Ack=1 |

**Requisição GET (filtro `http`).** O `GET /` carrega os parâmetros na própria URL e não traz corpo (payload zerado): só os cabeçalhos de metadados do cliente.

**Requisição POST e o achado central (filtro `http.request.method == "POST"`).** Ao enviar um formulário de teste para `httpbin.org`, expandi o corpo da requisição e confirmei: **os campos trafegaram em texto claro**. Um campo preenchido com uma senha fictícia apareceu legível no pacote.

| Campo | Valor interceptado | Status |
|---|---|---|
| custname | "Aluno Teste" | exposto |
| custemail | "aluno@teste.com" | exposto |
| comments | "SENHA-SECRETA-123" | **crítico — senha em claro** |

**Reconstrução da sessão (Follow TCP Stream).** Remontei o fluxo TCP completo em ordem cronológica e vi a requisição inteira, incluindo o corpo `application/x-www-form-urlencoded` com os dados do formulário. Uma busca global (`frame contains "SENHA-SECRETA"`) localizou a senha diretamente no payload.

## Leitura de segurança

A captura responde às três perguntas da atividade:

- **GET × POST não é sobre segurança.** O GET expõe os parâmetros na URL e o POST os leva no corpo, mas a captura comprovou que **ambos trafegam em texto claro sob HTTP simples**. Esconder o dado da barra de endereço não o protege na rede.
- **Por que HTTP em formulário de login é risco crítico.** Em rede compartilhada ou pública (Wi-Fi), qualquer um com um sniffer ou em posição de *man-in-the-middle* lê senha, dado bancário e informação pessoal direto do payload. É perda total de confidencialidade.
- **Como o HTTPS resolve.** O TLS estabelece uma camada cifrada sobre o TCP antes da troca de dados da aplicação. Na mesma captura sob HTTPS, o Wireshark mostraria apenas blocos ininteligíveis no campo *Application Data* — a senha seria ilegível sem a chave privada correspondente.

## Conclusão

Ver uma senha aparecer em claro num Follow TCP Stream é o argumento mais direto a favor de TLS em tudo — mais convincente que qualquer recomendação escrita. É a razão pela qual, no sistema que mantenho, nada sensível trafega sem cifragem nem é validado no cliente: o que anda na rede, um dia alguém captura, e a única proteção que sobrevive a essa captura é a criptografia.

## Filtros utilizados

| Filtro | Objetivo |
|---|---|
| `dns` | isola consultas e respostas DNS |
| `ip.addr == X.X.X.X` | todo o tráfego de origem ou destino de um IP |
| `tcp.flags.syn == 1` | tentativas de abertura de conexão TCP |
| `http.request.method == "POST"` | pacotes HTTP que enviam formulário |
| `frame contains "texto"` | busca um termo no payload dos quadros |

## Evidências

As capturas de tela do Wireshark (DNS, handshake, GET, POST em claro e Follow TCP Stream) acompanham o relatório completo entregue na disciplina.
