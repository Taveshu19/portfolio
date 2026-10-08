# Leitura de notas com IA

Em outubro de 2026 o Gemini virou o leitor principal dos PDFs do painel. Ele devolve número da nota, fornecedor, CNPJ, valor, datas, linha digitável do boleto, dados bancários e o tipo de documento.

Colocar a IA para ler foi a parte fácil. O trabalho de verdade foi saber quando dá para confiar no que ela leu.

## Como a chamada funciona

O painel é uma página pública, então a chave da API não pode ficar nele. O painel manda o PDF para uma função no Supabase, que só aceita quem está logado, e é essa função que chama o modelo.

A resposta vem num formato fixo, definido por um JSON Schema: tipo de documento, natureza e forma de pagamento só podem ser um dos valores da lista, e o que não existe no documento volta como vazio. No prompt eu explico as armadilhas que vi acontecer: a empresa compradora nunca é o fornecedor; o número da nota não é a chave de acesso nem o RPS; em nota de serviço vale o valor bruto, antes dos impostos. E se o modelo não tiver certeza de algum campo, ele tem que dizer, numa lista à parte.

A chamada passa pelo OpenRouter, com um segundo modelo de reserva caso o primeiro falhe, novas tentativas só para erros passageiros, e a opção que restringe a provedores que não guardam nem treinam com os documentos. Trocar de modelo é mudar uma configuração.

## Como eu confiro o que ela leu

A IA erra, e erra com confiança. Então o painel confere:

- se o dígito verificador do CNPJ bate;
- se o número e o valor que ela leu aparecem mesmo no texto do PDF;
- se o valor bate com a linha digitável do boleto, que é a fonte mais confiável;
- quais campos o próprio modelo disse que não tinha certeza;
- se ela reconheceu o arquivo como nota ou boleto.

Qualquer problema vira um aviso na linha ("Gemini · conferir", com o motivo) e a nota fica marcada para revisão. A meta é que uma nota sem aviso possa ser lançada sem abrir o PDF.

## Se a IA não responder

O painel volta para o leitor antigo: XML quando existe, depois o texto do PDF, depois OCR. E o que qualquer um dos leitores leu fica guardado separado do que a equipe corrigiu, para eu conseguir medir quanto cada um acerta.

## Próximo passo

Usar a classificação do Gemini para tirar automaticamente os arquivos que não são notas e que hoje a equipe apaga à mão. Antes disso quero algumas semanas de uso mostrando que ele não descarta nenhuma nota de verdade.
