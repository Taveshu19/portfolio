# Integração com o ERP TOTVS Datasul

Depois de aprovada na Monday, cada nota ainda virava um pedido de compra digitado à mão no ERP. Comecei esse projeto para gerar o pedido automaticamente a partir da nota aprovada.

Hoje a geração funciona em ambiente de teste e o projeto está em piloto. A parte que roda dentro do ERP depende de liberações de acesso e de infraestrutura que não estão na minha mão, então ainda não está em produção.

## Antes de construir, medi

Não queria começar com um número inventado. Analisei as 3.234 notas do histórico:

- 2.825 tiveram o pedido digitado à mão;
- em 23% delas (faturas, CT-e e remessas) não existe nenhuma decisão humana, então tudo pode sair automático;
- nas outras, a pessoa só precisa escolher o item, e o resto do pedido se preenche;
- 99,3% dos CNPJs já existem no cadastro do ERP.

Chegou a aparecer a estimativa de "95% da digitação eliminada". Tirei, porque não tinha medição nenhuma por trás.

## O que foi feito

O sistema pega as notas aprovadas na Monday, encontra o fornecedor pelo CNPJ (e pergunta quando há mais de um cadastro), sugere o item a partir do que já foi escolhido antes para aquele fornecedor e gera o arquivo de importação no layout que o ERP espera, com a codificação, as quebras de linha e os totais em centavos exatamente como ele exige.

O número do pedido é reservado no banco antes de liberar o arquivo, para dois lotes nunca usarem o mesmo número, e só volta para a Monday depois de conferido. Um agente na máquina da empresa grava o arquivo na pasta de rede que o ERP lê.

## O que aprendi

Nem tudo dá para automatizar até o fim. Para o passo que cria o pedido dentro do ERP, a recomendação foi aceitar "um clique por dia para o lote inteiro" em vez de um robô de tela que quebraria a qualquer mudança.
