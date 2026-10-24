# Atividade 01 — Reconhecimento e Footprinting

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Reunir o máximo de informação sobre um alvo autorizado antes de iniciar qualquer teste, distinguindo o que é possível obter sem interagir com o alvo (reconhecimento passivo) do que exige contato direto (reconhecimento ativo). Os alvos foram `nmap.org` e `scanme.nmap.org`, hosts públicos que o próprio projeto Nmap disponibiliza para fins didáticos; nenhuma varredura intrusiva foi realizada fora desse escopo.

## Fundamento

O reconhecimento é a etapa inicial de um teste de intrusão e, ao mesmo tempo, a primeira oportunidade de defesa. Toda informação obtida nesta fase corresponde a uma superfície que a organização expôs. O reconhecimento passivo apoia-se em fontes de terceiros (OSINT) e praticamente não deixa rastros; o ativo envia estímulos ao alvo e, por isso, fica registrado em logs, firewalls e IDS/IPS.

Classifiquei quatro ações quanto a esse critério: consultar o LinkedIn da empresa é passivo, pois ocorre na plataforma e não na infraestrutura do alvo; executar `ping` é ativo, por enviar pacotes ICMP diretamente ao host; a engenharia social por telefone é ativa, pela interação direta com um colaborador; e consultar o histórico do site no Wayback Machine é passivo, pois o conteúdo vem do acervo do Internet Archive.

## Execução

Trabalhei em um Ubuntu Server em instância AWS EC2 (região `us-east-1`). A sequência abaixo vai da consulta de registro até a correlação entre as fontes.

**Registro do domínio (WHOIS).** Consultei a base de registro para obter o registrador e os servidores autoritativos:

```bash
whois nmap.org
```

O domínio está registrado na Dynadot Inc. (IANA ID 472), criado em 18/01/1999, com resolução delegada ao Linode (`ns1` a `ns5.linode.com`, o que indica redundância). Registrei dois pontos de postura: o status `clientTransferProhibited`, que impede transferência não autorizada entre registradores, e `DNSSEC: unsigned`, ou seja, a zona não possui assinatura DNSSEC.

**Resolução de nomes (nslookup).** Forcei o resolvedor público da Cloudflare (`1.1.1.1`) para que a resposta refletisse o que a internet enxerga, e não um cache interno da VPC:

```bash
nslookup -type=A scanme.nmap.org 1.1.1.1
nslookup -type=MX nmap.org 1.1.1.1
```

O registro A de `scanme.nmap.org` retornou `45.33.32.156`. O MX preferencial (prioridade 1) é `ASPMX.L.GOOGLE.COM`, o que mostra dependência de Google Workspace para e-mail.

**Consulta avançada (dig).** Usei o `dig` para auditar o TTL e os registros TXT ligados à autenticação de e-mail:

```bash
dig scanme.nmap.org A
dig nmap.org TXT
```

O TTL do registro A é de 300 segundos. O registro TXT contém a política SPF, encerrada em `~all` (softfail): origem não autorizada é apenas marcada como suspeita, não rejeitada. Uma política `-all` (hardfail) seria mais restritiva, e o mecanismo `ptr` presente é desaconselhado pela RFC 7208.

**Descoberta de subdomínios e serviços (OSINT).** Pelo DNSDumpster, de forma passiva, identifiquei o host `ack.nmap.org` em `50.116.1.184`, hospedado na Akamai/Linode (ASN 63949). Esse mesmo IP é o autorizado no mecanismo `ip4` do SPF levantado com o `dig`: duas fontes independentes confirmam que o host participa da infraestrutura de e-mail do domínio. Os banners agregados apontavam versões antigas (Apache 2.4.6/2.4.7, OpenSSH 6.6.1 e 7.4), que em um pentest real seriam prioridade de verificação.

## Dificuldades

Na etapa do Shodan, a ficha que coletei correspondia ao IP `1.1.1.1` — o resolvedor DNS usado nas consultas — e não ao alvo `45.33.32.156`. A distinção é relevante: o resolvedor é apenas o intermediário da consulta, enquanto o alvo do reconhecimento é o host que responde pelo nome pesquisado. Registrei o escopo correto para refazer a consulta ao IP do alvo e anexar como complemento.

## Conclusão

A sequência de técnicas mostra como dados isolados se transformam em um mapa coerente da superfície de ataque: o WHOIS entregou o registrador e a delegação ao Linode, o `nslookup` e o `dig` confirmaram o endereço e expuseram a política SPF em softfail, e o DNSDumpster ligou um subdomínio ao mesmo IP autorizado no SPF, fechando a correlação entre DNS e infraestrutura de correio.

Do ponto de vista defensivo, boa parte desse levantamento ocorre sem qualquer interação com o alvo, portanto não há log a inspecionar nem alerta a disparar nessa fase. A mitigação está em reduzir o que se publica: revisar registros TXT desnecessários, endurecer a política SPF para `-all`, avaliar a adoção de DNSSEC — hoje ausente no domínio — e manter fora do DNS público nomes que denunciem ambientes internos.

## Evidências

As capturas de tela da execução (saídas do WHOIS, do `nslookup`, do `dig` e das consultas OSINT) acompanham o relatório completo entregue na disciplina.
