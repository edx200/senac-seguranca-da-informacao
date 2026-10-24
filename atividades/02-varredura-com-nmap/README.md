# Atividade 02 — Varredura com Nmap

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Mapear as portas abertas, os serviços e as versões em execução em um host autorizado, comparando o que cada modalidade de varredura revela e o ruído que produz. O alvo foi `scanme.nmap.org` (`45.33.32.156`), host mantido pelo projeto Nmap e expressamente disponibilizado para testes; nenhum outro sistema foi varrido. Ambiente: Ubuntu Server em AWS EC2 (`us-east-1`), Nmap 7.98.

## Fundamento

O Nmap opera por reconhecimento ativo: envia pacotes ao alvo e infere o estado dos serviços a partir das respostas. Diferente do OSINT passivo, toda execução deixa rastro em logs, firewalls e IDS, e por isso exige autorização prévia. Os parâmetros que usei: `-sS` (SYN scan, half-open, exige root), `-sT` (TCP connect, padrão sem root), `-sV` (versão dos serviços), `-O` (sistema operacional por fingerprint), `-A` (agressivo: combina `-sV`, `-O`, `-sC` e traceroute), `-p` (portas) e `-T0`–`-T5` (temporização).

## Execução

**Descoberta básica.** A varredura padrão encontrou quatro portas abertas: `22/tcp` (ssh), `80/tcp` (http), `9929/tcp` (nping-echo) e `31337/tcp`. A `25/tcp` (smtp) ficou `filtered`, indicando bloqueio por firewall sem resposta conclusiva.

```bash
nmap scanme.nmap.org
```

Observei que, apesar de o enunciado chamar a varredura padrão de SYN scan, sem `sudo` o Nmap recorre ao TCP Connect Scan. A evidência está no motivo das portas fechadas: `conn-refused` (retornado pela syscall `connect()`), e não `reset`.

**SYN scan com privilégio.** O conjunto de portas foi idêntico, mas o mecanismo mudou:

```bash
sudo nmap -sS scanme.nmap.org
```

Com o `-sS`, o Nmap envia apenas o SYN e aborta o handshake com RST ao receber SYN-ACK. O motivo das portas fechadas passou de `conn-refused` para `reset`, confirmando a leitura direta do pacote.

**Detecção de versão e de SO.**

```bash
nmap -sV scanme.nmap.org
sudo nmap -O scanme.nmap.org
```

Serviços: OpenSSH 6.6.1p1 (`22`), Apache httpd 2.4.7 Ubuntu (`80`), Nping echo (`9929`) e `tcpwrapped` (`31337`). O SO estimado foi Linux 4.19–5.15. O custo da detecção de versão foi alto: o tempo saltou de ~2 s para 8,99 s, por abrir conexão completa e enviar probes a cada porta.

**Scan agressivo e portas específicas.**

```bash
sudo nmap -A scanme.nmap.org
nmap -p 22,80,443 scanme.nmap.org
```

O `-A` reuniu tudo em 20 s e acrescentou o que as técnicas isoladas não trouxeram: as quatro chaves públicas SSH (`ssh-hostkey`), cabeçalhos HTTP e o traceroute de 13 saltos pela rede da Akamai. A varredura restrita a três portas custou 0,16 s e confirmou `443/tcp closed` — o host serve só HTTP, sem TLS.

| Comando | Tempo | Informação | Ruído |
|---|---|---|---|
| `nmap` (padrão, `-sT`) | 2,04 s | 4 portas abertas, 1 filtrada | Médio |
| `sudo nmap -sS` | 2,18 s | mesmas portas, sem completar handshake | Baixo |
| `nmap -sV` | 8,99 s | versões de SSH, HTTP, Nping | Alto |
| `sudo nmap -O` | 3,75 s | Linux 4.19–5.15; 13 hops | Médio |
| `sudo nmap -A` | 20,03 s | tudo acima + chaves SSH, HTTP, traceroute | Muito alto |
| `nmap -A` (sem root) | 13,89 s | versões e scripts; sem SO e sem traceroute | Alto |
| `nmap -p 22,80,443` | 0,16 s | 22 e 80 abertas; 443 fechada | Muito baixo |

## Dificuldades

O achado mais instrutivo foi acidental: a mesma flag `-A` produziu dois relatórios diferentes conforme houvesse ou não `sudo`. Sem privilégio, desapareceram a detecção de SO e o traceroute, porque ambos dependem de raw sockets, operação restrita ao root. A lição prática é que a ausência de um dado no relatório pode indicar limitação do operador, e não ausência da informação no alvo — por isso toda evidência precisa registrar o comando exato e o contexto de execução.

Outro ponto que exigiu cuidado foi a contradição entre o banner do SSH (Ubuntu 14.04, kernel 3.13) e o fingerprint (kernel 4.19–5.15): o banner é texto declarado pela aplicação, o fingerprint é medido na pilha TCP/IP. Confrontar as duas fontes é o que evita conclusões precipitadas.

## Conclusão

Varredura não é um comando único, e sim uma escala de compromisso entre profundidade de informação e visibilidade para o alvo. A varredura de três portas custou 0,16 s e devolveu um dado pontual; o scan agressivo custou 20 s e devolveu um retrato quase completo do host. A escolha é de objetivo, não de preferência. Mesmo a fama de furtivo do `-sS` merece ressalva: ele é invisível para a aplicação, não para um IDS moderno.

Do lado defensivo, o alvo ilustra o custo de expor serviços desatualizados: Apache 2.4.7 sem TLS, OpenSSH 6.6.1p1 com chave DSA de 1024 bits depreciada. As medidas diretas seriam atualizar os serviços, habilitar HTTPS, remover algoritmos depreciados e restringir por firewall as portas que não precisam estar acessíveis, reduzindo a superfície antes que ela seja inventariada por terceiros.

## Evidências

As dez capturas de tela das varreduras acompanham o relatório completo entregue na disciplina.
