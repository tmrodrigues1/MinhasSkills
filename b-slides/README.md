# B Slides

Uma skill para criar apresentações em HTML locais e portáteis a partir de um briefing. O deck abre no navegador sem servidor e mantém suas dependências na própria pasta.

## Como usar

Disponibilize a pasta `b-slides` ao seu agente de IA e peça, por exemplo:

> Use a skill b-slides para criar uma apresentação sobre estratégia de produto para o time interno.

A skill conduz um briefing sobre objetivo, público, conteúdo e identidade visual. Em seguida, apresenta um outline para aprovação antes de gerar o deck. Você também pode pedir para usar os padrões ou delegar as escolhas ao agente.

Forneça os arquivos ou caminhos locais de logos, fontes, imagens e templates que quiser usar. A skill não inclui assets de marca.

## Recursos

- Slides em canvas 16:9, com escala para diferentes tamanhos de janela.
- Navegação pelo teclado, contador de slides e modo tela cheia.
- Alternância entre temas claro e escuro e suporte a movimento reduzido.
- Layouts para capas, comparações, destaques, conteúdo estruturado e painéis.
- Estilos de impressão e um `brief.md` para registrar as escolhas aprovadas.

## Estrutura

```text
b-slides/
├── README.md
└── SKILL.md
```

O deck gerado contém `index.html`, `brief.md` e uma pasta `assets/` com os arquivos locais necessários. Consulte o `SKILL.md` para conhecer o fluxo e os requisitos completos. A configuração e a compatibilidade de skills dependem do agente utilizado.
