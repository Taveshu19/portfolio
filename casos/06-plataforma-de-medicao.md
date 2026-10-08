# Plataforma de Medição de Empreiteiros

Projeto pessoal que faço com o Pedro, que trabalha com obras. Todo mês cada empreiteiro manda a medição de um jeito, e a engenharia acaba montando a planilha por ele. A plataforma junta o ciclo todo num lugar: contrato, medição, aprovação, nota fiscal e histórico.

O empreiteiro lança pelo celular, a engenharia aprova no computador e a nota só é liberada depois da aprovação final.

O código é aberto: [Taveshu19/plataforma-medicao](https://github.com/Taveshu19/plataforma-medicao). Lá no README eu explico as decisões com mais calma. As principais:

- várias construtoras no mesmo sistema, e quem separa os dados de cada uma é o próprio banco (Row Level Security);
- saldo e mudança de status calculados no banco, não na tela, porque o celular do empreiteiro não é confiável;
- saldo sempre calculado, nunca guardado, contando o que ainda está em análise, para ninguém medir a mesma coisa duas vezes.

Feito em Next.js, React, TypeScript e Supabase, com 31 migrações de banco, testes com Vitest e Playwright, e uma demonstração no Vercel.
