# Como eu desenvolvo

Eu não uso IA como chat. Trabalho pelo terminal, com vários agentes de código ao mesmo tempo, cada um com uma tarefa, um contexto e regras. Foi assim que consegui fazer sozinho, em poucos meses, os projetos deste portfólio.

## Vários agentes, cada um no seu canto

Uso o Orca para coordenar o Claude Code, o Codex, o Antigravity, o OpenCode e o Hermes. Cada agente trabalha numa cópia própria do repositório (git worktree), então um não atropela o trabalho do outro.

Escolho o modelo pelo tipo de tarefa. Para banco, regra de negócio e integração, uso os mais fortes (Claude Opus e GPT Astra). Para telas e testes em cima de uma regra que já está pronta, uso os mais rápidos e baratos (Claude Sonnet e Gemini Flash).

E uma regra que sigo: quem implementa não revisa. Um agente escreve a partir de uma especificação, outro, de outro fornecedor, revisa a especificação e o código. Só um escreve por vez em cada repositório, e eu confiro antes de juntar.

## Contexto

Cada projeto tem arquivos de instruções versionados (CLAUDE.md e AGENTS.md) com o porquê das decisões que não são óbvias. Isso evita que o próximo agente "simplifique" justamente o trecho que segura o sistema.

Uso hooks para automatizar o que é repetitivo: no painel de notas, cada edição já publica sozinha no Netlify. Os agentes acessam Monday.com, Supabase, Microsoft 365, Linear e GitHub por MCP. E cuido da janela de contexto e da memória das sessões, com pontos de restauração e controle do consumo de tokens.

## Organização

O Obsidian é onde fica a verdade: cada projeto tem uma pasta de pendências em Markdown, com tipo, status, prioridade e responsável. O Linear é o espelho, para eu ver em que pé está cada coisa. Escrevi o script que sincroniza os dois sozinho e que monta a estrutura de cada projeto novo no Orca.
