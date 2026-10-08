# Um assistente que consulta o banco

Toda dúvida sobre uma nota acabava chegando em mim: "o que aconteceu com a nota 1234?", "fui eu que excluí?", "já foi para a Monday?". Para responder eu abria o banco e ia atrás do histórico. Então coloquei o Claude dentro do painel para fazer essa investigação.

A pessoa pergunta em português e ele responde em português simples, dizendo de onde tirou cada informação.

## Como funciona

É um agente com ferramentas, rodando numa função do Supabase. Para cada pergunta ele pode dar até 10 passos usando quatro ferramentas:

- **rastrear_nota:** monta a linha do tempo de uma nota, com quando chegou, quem mexeu, se foi excluída e se foi para a Monday;
- **consultar_banco:** faz uma consulta livre, só de leitura, com limite de tempo e de linhas;
- **consultar_monday:** confere se o item existe na Monday e quais arquivos tem;
- **propor_mudanca:** prepara uma correção para uma pessoa aprovar. Ela não executa nada.

## O que impede ele de fazer besteira

Eu não quis depender do prompt para isso. As travas estão no banco.

As consultas rodam com um usuário do Postgres que só pode ler. Mesmo que o código tenha algum erro, ele não consegue passar disso.

Para corrigir alguma coisa, o assistente monta uma proposta. O sistema confere se ela só mexe nas tabelas permitidas, roda a mudança num ensaio que é desfeito no final e mostra para a pessoa o antes e o depois. Só quando ela clica em Confirmar a mudança acontece de verdade, com outro usuário do banco, e um gatilho guarda como a linha estava, para dar para desfazer depois. A proposta vence em 15 minutos e não roda duas vezes se alguém clicar duas vezes.

## O que impede ele de inventar

As instruções dele dizem para nunca responder de memória: investigar primeiro e citar a fonte. "Não encontrei" é uma resposta aceitável; resposta errada não.

Um problema que apareceu: quando alguém perguntava "o que entrou nos últimos 20 minutos?" duas vezes, em horários diferentes, os números mudavam e o modelo pedia desculpas por um erro que não existia. Resolvi guardando, junto com cada resposta, a hora e as consultas que ele fez. Assim ele sabe que eram períodos diferentes.

## Custo

O assistente usa uma chave separada com limite de gasto, temperatura zero e cache do prompt. Cada conversa fica registrada com o custo, os passos e as propostas.
