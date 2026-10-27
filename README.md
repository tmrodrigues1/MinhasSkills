# Minhas Skills

Esta pasta reúne skills criadas por [Thiago Rodrigues](https://thiagorodrigues1.com.br/): conjuntos de instruções que orientam agentes de IA a executar tarefas específicas de forma consistente.

## Skills disponíveis

- [B Slides](b-slides/README.md) — cria apresentações em HTML locais e portáteis a partir de um briefing.
- [Jogo Ito](jogo-ito/README.md) — gera temas subjetivos para jogar no estilo de Ito.

Cada pasta contém um arquivo `SKILL.md` com as instruções da skill e um README com informações sobre seu uso. Para utilizá-las, disponibilize a pasta ou as instruções ao seu agente de IA, conforme o formato e o processo de configuração aceitos pela ferramenta escolhida.

## Como instalar

Baixe ou clone este repositório e, em um terminal aberto na pasta baixada, copie a pasta da skill desejada para o diretório correspondente. Os exemplos abaixo usam `b-slides`; substitua o nome para instalar outra skill.

### Claude Code

Para disponibilizar a skill em todos os seus projetos:

```bash
mkdir -p ~/.claude/skills
cp -R b-slides ~/.claude/skills/
```

Para instalá-la somente em um projeto, copie-a para `.claude/skills/` na raiz desse projeto. No Claude Code, invoque-a com `/b-slides` ou peça que o agente a utilize. Consulte a [documentação de skills do Claude Code](https://code.claude.com/docs/en/skills).

### Codex

Para disponibilizar a skill em todos os seus projetos:

```bash
mkdir -p ~/.agents/skills
cp -R b-slides ~/.agents/skills/
```

Para instalá-la somente em um projeto, copie-a para `.agents/skills/` na raiz desse projeto. Inicie uma nova sessão e peça ao Codex para usar `b-slides` ou invoque-a com `$b-slides`. Consulte a [documentação de skills da OpenAI](https://developers.openai.com/codex/skills/).

## Autor
[Thiago Rodrigues](https://thiagorodrigues1.com.br/)


