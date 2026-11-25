# Testes do prompt

IA usada: Claude (Anthropic)
Data: 25/11/2026

Fiz os três testes que o desafio pede. O primeiro mostrou um problema, eu corrigi o prompt e repeti. Os testes 2 e 3 já foram com a versão corrigida.

## Teste 1: o achado do jeito que eu descrevi

O que eu mandei: o prompt inteiro, na versão 1.

O que eu queria ver: se ela separava evidência de hipótese e se seguia a ordem dos 5 itens.

O que aconteceu: ela não fez a análise. Como a versão 1 terminava com "pergunte antes de responder", ela respondeu só com perguntas: se o root tem MFA, se o sócio estava viajando no sábado e se a chave nova já tinha sido usada. As perguntas eram boas, mas eu fiquei sem saber o que fazer naquela hora.

O que eu mudei: troquei a última linha. Agora a IA responde com o que tem, mostra onde a falta de dado pesa e deixa as perguntas pro fim.

Repeti o teste com a versão 2 e dessa vez veio tudo na ordem:
- as hipóteses vieram marcadas como hipótese: credencial vazada, uso pelo próprio sócio e alguém de dentro com a senha;
- as evidências diziam onde procurar: no CloudTrail, os eventos feitos pelo root, com o IP de origem e se o login usou MFA; no IAM, o relatório de credenciais, que mostra se o root tem MFA e quando a chave foi usada;
- na contenção, separou desativar a chave (dá pra voltar atrás) de apagar a chave (não dá), com o impacto de cada uma;
- as perguntas ficaram no fim.

## Teste 2: pergunta que ela não tem como responder

O que eu mandei, depois da resposta do teste 1:

> De qual país veio o acesso? E qual é o ID da conta?

O que eu queria ver: se ela admitia que não tem esses dados ou se inventava um país e um número de conta.

O que aconteceu: ela disse que não tinha os dois dados, porque eu só falei "IP fora do Brasil" e não passei o ID da conta. E ainda disse onde achar: o país aparece nos detalhes do IP dentro do próprio achado do GuardDuty, e o ID da conta também está no achado e no menu da conta, no canto superior direito do console.

O que eu mudei: nada. A regra de não inventar funcionou.

## Teste 3: caso ambíguo

O que eu mandei: o mesmo prompt, com mais esta linha no contexto do ambiente:

> Na sexta-feira, o sócio fundador avisou no grupo que ia fazer uma migração de serviços no fim de semana.

O que eu queria ver: se ela cravava "uso legítimo" ou se pedia a evidência que confirma.

O que aconteceu: ela subiu a hipótese de uso legítimo para o primeiro lugar, mas não fechou o caso. Disse que um aviso no grupo não prova quem fez a chamada e pediu duas confirmações: perguntar direto ao sócio e comparar o IP e o horário do CloudTrail com onde ele estava. Também lembrou que, mesmo sendo ele, deixar uma chave de acesso no root é arriscado, e sugeriu apagar a chave depois da migração e usar um usuário do IAM no lugar.

O que eu mudei: nada. A regra de não concluir sem evidência segurou bem.

## O que eu aprendi

- Duas regras que parecem boas podem brigar entre si. "Pergunte antes de responder" e "diga o que falta em vez de preencher" puxavam pra lados diferentes, e só o teste mostrou isso.
- A IA admitir que não sabe é só metade. O mais útil foi ela dizer onde achar o dado, e isso veio do item 3 do prompt, que pede pra dizer onde encontro cada evidência.
- Severidade baixa não quer dizer tranquilo. O que pesou na investigação foi o contexto: root, chave nova, IP de fora e horário estranho.