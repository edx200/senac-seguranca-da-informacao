# Prática — Criptografia de Disco (dados em repouso)

**Natureza:** prática pessoal de hardening, autodirigida (não é atividade de disciplina).

---

## Objetivo

Proteger os dados em repouso das minhas máquinas de trabalho por criptografia de disco completa, tanto no Windows quanto no Linux. A motivação é concreta: desenvolvo e mantenho um sistema em produção, e nas estações de trabalho existem segredos e dados que não podem vazar caso um equipamento seja perdido, roubado, descartado ou acessado fisicamente.

## Fundamento

Criptografia em trânsito (TLS) protege o dado enquanto ele viaja; **criptografia em repouso** protege o dado parado no disco. São problemas diferentes, e um não substitui o outro. A criptografia de disco completa (full-disk encryption) atende a três cenários que a senha de login do sistema operacional não cobre:

- **Equipamento perdido ou roubado** — sem a chave, o conteúdo do disco é ruído, mesmo removido e ligado em outra máquina.
- **Descarte de mídia** — um SSD aposentado não entrega dados residuais.
- **Acesso físico** — a senha do Windows ou do Linux é contornável por quem tem o disco em mãos; a criptografia do volume, não.

O elemento que fecha essa proteção é a **autenticação pré-boot**: a chave é exigida antes do sistema operacional carregar, de modo que nada do disco é legível até a senha correta.

## Windows — VeraCrypt (system encryption)

Apliquei a criptografia da partição de sistema com o **VeraCrypt**, em **AES**, com autenticação pré-boot. A senha passa a ser pedida numa tela própria, antes do Windows iniciar.

A escolha do VeraCrypt, em vez do BitLocker, foi técnica: em um Windows Home sem TPM nem Secure Boot disponíveis, o BitLocker não é uma opção prática, e o VeraCrypt oferece a mesma criptografia de partição de sistema sem depender desse hardware.

Dois cuidados que a prática ensinou:

- **Mídia de resgate é obrigatória.** A criptografia de sistema grava um carregador próprio; se ele corromper, o disco de resgate é o único caminho de volta. Ele é gerado no processo e deve ser guardado em local separado e offline.
- **A tela de pré-boot usa layout de teclado US.** Uma senha definida com o teclado ABNT2 pode não ser reconhecida no boot se contiver símbolos que mudam de posição entre os layouts. Vale definir e conferir a senha pensando no layout do pré-boot.

## Linux — LUKS

No Linux (Kali, em SSD externo dedicado às práticas), a criptografia de disco é feita com **LUKS**, o padrão nativo do ecossistema para volumes cifrados, também com autenticação no boot. Manter o ambiente ofensivo em um disco separado e cifrado isola essas práticas do restante do trabalho.

## Boas práticas de gestão

- **Senha forte e única** na autenticação pré-boot — é a única barreira real; se for fraca, o resto não importa.
- **Mídia de resgate offline**, longe da máquina que ela resgata.
- **Segredos sensíveis em volume separado** do sistema, para reduzir o que fica exposto quando o sistema está montado e em uso.

## Conclusão

Criptografia de disco é a diferença entre um notebook perdido ser um transtorno e ser um incidente. Para quem guarda segredos de produção e dados de cliente na própria estação, ela deixa de ser recomendação e vira requisito: é o controle que garante que o acesso físico ao hardware não se converta em acesso aos dados. Aplicá-la nas duas plataformas que uso, Windows e Linux, foi a forma de tornar esse requisito real, e não teórico.
