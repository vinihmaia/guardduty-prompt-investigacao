# Desafio Criativo DIO: do alerta à resposta

Esse repositório é a minha entrega do Desafio Criativo da trilha AWS Cibersegurança da DIO. A proposta era montar um prompt que pegasse um achado do Amazon GuardDuty e transformasse numa investigação de verdade, com hipóteses, evidências pra checar e uma resposta que dê pra justificar.

[Uma ou duas frases sobre você. Ex.: trabalho com TI e estou começando em segurança na nuvem.]

## O caso que escolhi

Os exemplos do desafio usavam força bruta em SSH, então resolvi ir por outro lado e pegar um achado de identidade: `Policy:IAMUser/RootCredentialUsage`, que aparece quando alguém usa as credenciais do usuário root da conta.

O que me chamou atenção é que o GuardDuty marca esse achado como severidade baixa. Faz sentido, porque às vezes o próprio dono da conta usa o root. Só que o root pode fazer qualquer coisa na conta. No meu cenário, a chamada foi um CreateAccessKey, de um IP de fora do Brasil, num sábado à noite. Se a IA olhar só pro rótulo "baixa", ela erra feio. Foi isso que eu quis testar.

O cenário é inventado. Nenhum dado aqui é de conta real.

## Como o prompt está montado

O prompt está em [prompt-guardduty.md](prompt-guardduty.md) e tem quatro partes:

1. **O achado e quem vai ler.** Tipo, recurso, severidade, horário e o que o GuardDuty descreve. Também deixo claro que quem lê programa bem mas nunca mexeu com CloudTrail, porque isso muda o jeito de explicar.
2. **O contexto do ambiente.** O que eu consigo confirmar e, principalmente, o que eu ainda não verifiquei. Sem essa parte a IA preenche o que falta por conta própria.
3. **A ordem da resposta.** Primeiro o que o achado indica, depois hipóteses, evidências (dizendo onde procurar), contenção e o que falta. Separar hipótese de evidência foi o ponto principal.
4. **As regras.** Não inventar IP, conta, país nem evento do CloudTrail, marcar o que é hipótese e avisar o impacto antes de sugerir revogar uma credencial.

No fim tem uma linha que deixa a IA perguntar quando faltar dado. Pra mim essa é a parte mais importante: em segurança, uma resposta confiante e errada às três da manhã pode fazer alguém derrubar a coisa errada.

## Testes

Rodei o prompt em [nome da IA que você usou] e anotei o que aconteceu em [testes.md](testes.md).

## Como usar

Copie o prompt, troque os dados do achado e do ambiente pelos do seu caso e cole na IA que preferir. Se ela responder fora da ordem ou inventar dado, quase sempre é regra faltando no prompt.