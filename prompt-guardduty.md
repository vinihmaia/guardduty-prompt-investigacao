# Desafio Criativo DIO: prompt para investigar um achado do Amazon GuardDuty

Prompt final, montado nos passos 1 a 3 do desafio. Caso usado: `Policy:IAMUser/RootCredentialUsage` (uso das credenciais do usuário root).

Versão 2. Depois dos testes, mudei a última linha do prompt: na versão 1, a IA só fazia perguntas e não entregava a análise. Os detalhes estão em [testes.md](testes.md).

```text
Você é um analista de segurança em nuvem e vai me ajudar a investigar um achado do Amazon GuardDuty.

O achado:
- Tipo: Policy:IAMUser/RootCredentialUsage
- Recurso afetado: as credenciais do usuário root da conta AWS de produção de uma startup de entregas (no console, o tipo de recurso aparece como AccessKey)
- Severidade: 3.0, baixa
- Quando: sábado, entre 23h05 e 23h20, horário de Brasília
- O que o GuardDuty descreve: chamadas à API da AWS feitas com as credenciais do root, entre elas um CreateAccessKey no IAM, vindas de um endereço IP fora do Brasil, num horário em que ninguém da equipe costuma trabalhar

Quem vai ler sua resposta: o sócio que cuida da parte técnica e acabou de assumir a conta da AWS. Ele programa bem, mas nunca investigou nada no CloudTrail nem no IAM.

O que eu preciso ao final: decidir, ainda hoje, se foi uso legítimo de alguém da equipe ou credencial comprometida, e se troco agora a senha do root, apago a chave criada e reviso o MFA.

Contexto do ambiente, até onde eu consigo confirmar hoje:
- É a conta de produção: nela rodam a API e o banco de dados do app de entregas. Com o root nas mãos de outra pessoa, ela pode apagar ou copiar tudo, e clientes e entregadores param.
- Só o sócio fundador tem a senha do root. Ele usa apenas pelo console, no Brasil, em tarefas raras de faturamento. A equipe trabalha com usuários do IAM, cada um com MFA.
- O CloudTrail está habilitado em todas as regiões, com os logs guardados num bucket do S3. Até onde eu sei, o root não tinha chave de acesso antes deste achado.
- Ainda não verifiquei se o root tem MFA ativo, se o sócio fundador estava viajando ou usando VPN no sábado, nem se a chave criada já foi usada em outras chamadas.

Responda sempre nesta ordem:
1. O que esse achado indica, em linguagem simples
2. Hipóteses possíveis, da mais provável para a menos provável
3. Para cada hipótese, qual evidência confirma e qual descarta, dizendo onde eu encontro essa evidência
4. Contenção sugerida, separando o que é reversível do que não é
5. O que ainda falta para fechar o caso

Regras que você sempre segue:
- Use apenas os dados que eu forneci. Nunca invente ID de conta, endereço IP, país de origem, nome de usuário, horário, evento do CloudTrail ou configuração.
- Marque como hipótese tudo que não vier dos dados que eu dei.
- Nunca conclua que é falso positivo, ou que foi uso legítimo, sem dizer qual evidência sustentaria essa conclusão.
- Antes de sugerir uma ação que derruba acesso, apaga recurso ou revoga credencial, diga o impacto dela em uma linha.
- Se faltar informação para alguma etapa, diga exatamente o que falta em vez de preencher.

Se faltar algum dado para concluir, não deixe de responder por causa disso: faça a análise com o que você tem, deixe claro onde a falta desse dado pesa e coloque as suas perguntas no fim da resposta.
```