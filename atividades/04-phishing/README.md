# Atividade 04 — Phishing

**Disciplina:** Resiliência Cibernética

---

## Objetivo

Compreender, na prática e em ambiente controlado, a anatomia de um ataque de phishing para, a partir dela, sustentar a postura defensiva. O laboratório foi executado com finalidade estritamente educacional, em ambiente isolado na AWS, com dados fictícios e termo de uso responsável: nenhuma credencial real foi coletada, e nenhuma das técnicas se aplica a sistema real ou sem autorização.

> Esta página pública trata do reconhecimento do ataque e da defesa. O procedimento ofensivo do laboratório, executado em ambiente restrito, permanece no repositório privado da disciplina.

## Fundamento

O phishing explora a pessoa, e não o sistema. Consiste em uma mensagem ou página que simula uma fonte confiável para induzir um clique, a entrega de uma credencial ou a abertura de um anexo. É o vetor de acesso inicial mais frequente justamente por contornar as proteções técnicas: por mais robusto que seja o controle, ele é cedido voluntariamente por quem foi enganado.

Na simulação, uma página de autenticação falsa foi publicada em ambiente controlado, por trás de um proxy reverso, para observar em tempo real a mecânica da captura de uma tentativa de login com dados fictícios. O valor do exercício, do ponto de vista de quem defende, está em ver de perto com que facilidade um cenário de engenharia social é montado, e o quanto a defesa não pode depender apenas do usuário não errar.

## Reconhecimento do ataque

Os sinais que denunciam uma página ou mensagem de phishing, e que todo usuário deveria reconhecer:

- Remetente cujo endereço não corresponde ao domínio legítimo da organização.
- Urgência artificial, que pressiona a vítima a agir sem verificar.
- Link cujo destino real difere do texto exibido.
- Solicitação de credenciais fora do fluxo habitual de autenticação.
- Domínio visualmente parecido com o verdadeiro, com pequena variação de grafia.

## Defesa

As medidas que levantei, organizacionais e técnicas, em camadas:

- **Autenticação multifator (MFA)**, preferencialmente por chave física (FIDO2/WebAuthn), que resiste à reutilização de uma senha roubada: a credencial capturada, sozinha, não basta para autenticar.
- **Conscientização e treinamento periódico** dos usuários para identificar URLs suspeitas, domínios falsificados e cabeçalhos de e-mail.
- **Autenticação de e-mail por SPF, DKIM e DMARC**, somada à inspeção de reputação de URLs por um gateway de segurança.
- **Monitoramento de logs de acesso e revogação rápida de sessão** diante de qualquer indício de comprometimento.

## Conclusão

Compreender a anatomia do ataque é o pilar para construir defesas eficazes: a simulação mostrou que a página falsa é simples de publicar, o que desloca o peso da proteção para as camadas que não dependem do acerto do usuário. Entre todas, o segundo fator resistente a phishing é a que mais reduz o impacto, pois transforma uma credencial vazada em uma tentativa frustrada. É a mesma razão pela qual, no sistema que mantenho, a autenticação sensível nunca se apoia em um único segredo conhecido pelo cliente.

## Evidências

As capturas da execução em ambiente controlado acompanham o relatório completo entregue na disciplina, no repositório privado.
