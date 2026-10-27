---
name: b-slides
description: "Cria e ajusta apresentações como decks HTML locais e portáteis. Use quando o usuário pedir apresentação, slides, deck, slide em HTML ou transformar conteúdo em apresentação. Começa com briefing de perguntas, aprova o outline e solicita os assets ao usuário antes de criar a estrutura do deck."
---

# B Slides

Gere decks para qualquer tema a partir de um briefing. Fontes, logos, imagens e templates opcionais são fornecidos pelo usuário em cada chamada. Toda dependência do resultado deve ser local.

## 1. Estrutura global e estrutura de cada deck

A skill global chama-se `b-slides` e contém suas instruções em `SKILL.md`. Ela não inclui fontes, logos, imagens ou um template de marca. Os assets são fornecidos pelo usuário ao chamar a skill, por anexo ou caminho local, e pertencem somente ao deck correspondente.

Depois do briefing e da aprovação do outline, crie esta estrutura na pasta de saída:

```text
<slug-do-tema>-deck/
├── index.html
├── brief.md
└── assets/
    ├── template/
    │   ├── deck.css
    │   └── deck.js
    ├── fonts/
    └── logos/
        ├── logo-blue.png   # somente se fornecida nessa versão/formato
        └── logo-white.png  # somente se fornecida nessa versão/formato
```

Crie todas as pastas, mesmo quando `fonts/` ou `logos/` ficarem vazias. Preserve nomes, formatos e proporções dos assets fornecidos; os nomes de logo acima são exemplos de variantes clara e escura, não arquivos obrigatórios. Adicione `assets/images/` quando houver imagens. O HTML referencia o CSS e o JS em `assets/template/` por caminhos relativos.

Se o usuário fornecer um shell HTML/CSS/JS, adapte-o e mantenha suas dependências locais. Caso contrário, crie o shell do deck após a confirmação, usando o sistema de layouts e controles desta skill. O shell gerado fica no próprio deck e pode ser reaproveitado em revisões. Não solicite ao usuário um template como requisito para começar.

## 2. Fase 1: briefing obrigatório (sempre a primeira ação)

Ao ser acionado, o agente **não** copia o template, não inspeciona assets e não monta outline antes de entender a necessidade. A primeira resposta é o briefing.

### 2.1 Regras do briefing

- **Extraia antes de perguntar.** Leia o pedido de acionamento e marque como respondido tudo o que já veio nele (empresa, tema, cores, logo etc.). Se o pedido ou contexto já indicar um `brief.md` anterior, ofereça reaproveitar o contexto da empresa e a identidade visual. Leia-o após o usuário escolher reutilizá-lo; não faça uma busca de arquivos antes da primeira resposta de briefing. Pergunte só o que falta e nunca repita uma pergunta já respondida.
- **No máximo 3 rodadas:** contexto (perguntas abertas), necessidade e identidade visual (múltipla escolha).
- **Perguntas abertas vão em texto ou na ferramenta apropriada para respostas livres, respeitando as instruções do ambiente; as de múltipla escolha vão na ferramenta.** Use uma mensagem de texto normal para a rodada 1 quando as instruções do ambiente permitirem perguntas em texto; caso contrário, use a ferramenta de perguntas disponível. Nas rodadas 2 e 3, use perguntas estruturadas quando a ferramenta existir (ex.: `request_user_input_async` no Codex ou ferramenta equivalente; respeite o limite de perguntas e opções de cada ferramenta). Sem essa ferramenta, faça **uma única mensagem** por rodada, com as perguntas numeradas e as opções listadas.
- **Toda pergunta de múltipla escolha tem uma saída "Decida por você / use o padrão".** O usuário nunca fica travado.
- **Atalhos:** se o usuário disser "usa o padrão", "rápido", "decide você", ou se o pedido já trouxer um briefing completo, pule direto para a Fase 2 (confirmação) com os defaults da seção 5. As cores, que não têm default, são escolhidas pelo agente a partir do tema.
- **Idioma:** infira pelo idioma do pedido. Pergunte apenas se for ambíguo.

### 2.2 Rodada 1: contexto (perguntas abertas, em uma mensagem de texto)

1. **Empresa:** "Conte-me sobre a sua empresa: o que ela faz e qual é o propósito dela."
2. **Tema:** "Sobre o que é esta apresentação?"

Como usar a resposta sobre a empresa:

- Ela define o **tom de voz** da copy, o **vocabulário** do setor e os **exemplos** usados nos slides.
- Ela alimenta a **capa** (nome da empresa, kicker, assinatura) e o **wordmark**, quando não houver logo.
- Ela orienta a **sugestão de paleta e de estilo** na rodada 3, quando o usuário pedir que o agente decida.
- Ela fecha o **arco narrativo**: o propósito da empresa vira o fio condutor ou o takeaway final, quando fizer sentido para o tema.
- Se a apresentação não for de uma empresa (projeto pessoal, aula, trabalho acadêmico, ONG), adapte a pergunta: "Conte-me sobre você / o projeto / a organização: o que faz e qual é o propósito."
- Se a resposta for curta, siga com ela. Não faça perguntas de acompanhamento sobre a empresa; o que faltar vira inferência marcada na confirmação.

### 2.3 Rodada 2: necessidade

| # | Pergunta | Opções sugeridas |
|---|----------|------------------|
| 3 | **Objetivo:** o que a apresentação precisa provocar? | Informar/atualizar · Convencer/vender · Ensinar/treinar · Reportar resultados |
| 4 | **Público e formato:** quem assiste e como o deck será usado? | Projetado com apresentador (pouco texto) · Enviado para leitura (mais autoexplicativo) · Diretoria/executivos · Time interno · Clientes/evento |
| 5 | **Conteúdo-base:** de onde vem o material? | Vou colar/anexar o conteúdo · Tenho só tópicos soltos · Só tenho o tema, crie o conteúdo |
| 6 | **Tamanho:** quantos slides, aproximadamente? | Curto (5–7) · Médio (8–12) · Longo (13–20) · Decida pelo conteúdo |

> O formato (pergunta 4) define a densidade da copy: deck projetado leva headlines fortes e pouco texto; deck para leitura pode ter textos de apoio um pouco mais completos, sem virar parágrafo.

### 2.4 Rodada 3: identidade visual

| # | Pergunta | Opções sugeridas |
|---|----------|------------------|
| 7 | **Cores** | Tenho as cores da marca (informe os HEX: primária, secundária, terciária) · Sugira 3 paletas para o tema · Decida você com base no tema |
| 8 | **Tipografia** | Vou fornecer Bookerly Bold e Light · Tenho arquivos de outra fonte (.woff2/.ttf/.otf, informe o caminho) · Use fontes do sistema / decida você |
| 9 | **Logotipo** | Tenho logo (informe o caminho; o ideal é ter uma versão clara e uma escura, em SVG ou PNG transparente) · Sem logo / use o nome em texto (wordmark) · Decida você pelo contexto |
| 10 | **Estilo e tema inicial** | Corporativo sóbrio · Editorial elegante · Moderno/ousado · Minimalista técnico (+ abrir em light ou dark) |

Detalhes por resposta:

- **Assets por apresentação:** peça ao usuário os arquivos ou caminhos locais de fontes, logos, imagens e eventual template escolhidos no briefing. Anexos e caminhos já informados contam como respostas; não peça novamente. Não existe logo ou fonte embutida na instalação global.
- **Bookerly:** use Bold e Light somente se o usuário fornecer os arquivos. Se faltarem, proponha fontes do sistema e indique essa escolha na confirmação; não diga que Bookerly foi aplicada.
- **Logo ausente:** use wordmark se essa for a escolha do usuário ou se ele delegar a decisão. Se ele escolher uma logo específica, aguarde o arquivo ou uma decisão explícita de prosseguir com wordmark.
- **Assets obrigatórios pendentes:** solicite o que falta antes da geração. Não crie arquivos vazios que simulem fontes ou logos. Não baixe assets nem invente a marca.
- **Validação de arquivos:** depois de entender o briefing, confirme a existência dos caminhos e o formato dos assets escolhidos antes de mostrar a confirmação final. Não altere os originais; copie para o deck somente o que será utilizado.

- **"Sugira 3 paletas":** apresente 3 opções nomeadas, cada uma com primária, secundária e terciária em HEX e uma linha sobre o clima que ela passa. O usuário escolhe uma.
- **"Decida você":** escolha uma paleta coerente com a empresa, o tema, o público e o estilo, e mostre os HEX na confirmação para o usuário aprovar.
- **Cor única informada:** derive secundária e terciária (variação tonal + um acento complementar) e mostre o resultado na confirmação.
- **As cores não têm valor fixo na skill:** a paleta sempre sai deste briefing (informada, sugerida ou decidida pelo agente). Por isso, a pergunta de cores nunca é pulada em silêncio. Se o usuário pedir para usar o padrão em tudo, o agente escolhe a paleta pelo tema e a exibe na confirmação.
- **Fonte por nome sem arquivo** (ex.: "quero Inter"): explique que o deck não usa CDN, peça o arquivo local ou ofereça uma pilha de fontes do sistema equivalente. Nunca baixe a fonte sem autorização explícita.
- **Caminho de logo:** verifique se o arquivo existe antes de seguir. Se não existir, avise e pergunte de novo.

Perguntas opcionais (só faça se o contexto pedir): há imagens, gráficos ou dados para incluir? Existe pasta de saída preferida? Se não houver resposta, use `./<slug-do-tema>-deck/` no diretório atual e informe isso na confirmação.

## 3. Fase 2: confirmação (briefing + outline)

Antes de gerar, mostre numa única mensagem curta:

1. **Briefing consolidado:** empresa (resumo em uma linha: o que faz e propósito), tema, objetivo, público/formato, nº de slides, paleta (HEX, indicando se foi informada, sugerida ou decidida pelo agente), fontes (heading/corpo), logo(s), estilo, tema inicial, pasta de saída. Marque com "(padrão)" o que veio dos defaults.
2. **Outline proposto:** uma linha por slide com nº, layout (seção 10), título e ideia-chave. O slide 1 é sempre `cover`.
3. A pergunta: **"Gero assim ou quer ajustar algo?"**

Só avance para a Fase 3 depois do "ok". Ajustes pedidos aqui são aplicados ao briefing e ao outline sem reabrir a entrevista inteira.

## 4. Fase 3: produção

1. Crie a pasta do deck e sua estrutura da seção 1. Adapte o shell fornecido pelo usuário, se houver; caso contrário, crie o shell local e salve CSS/JS em `assets/template/`. Em revisões, adapte o shell existente.
2. Copie para a pasta `assets/` do deck final as fontes e logos **fornecidas pelo usuário e escolhidas no briefing**, para que o deck permaneça portátil.
3. Aplique o briefing como tokens CSS (seção 6).
4. Converta o conteúdo em slides seguindo o outline aprovado.
5. O slide 1 é sempre uma capa.
6. Varie a composição de slide para slide, preservando o mesmo sistema visual, escala de espaçamento, tipografia e controles.
7. Garanta que navegação, troca de tema, fullscreen e animações funcionem sem rede.
8. Salve um `brief.md` na pasta do deck com o briefing aprovado para que o deck possa ser regenerado ou estendido depois com as mesmas escolhas.
9. Aplique o checklist final (seção 13) antes de entregar. Não use Playwright ou automação semelhante sem solicitação explícita do usuário; relate quais verificações puderam ser realizadas e quais ficaram pendentes, sem afirmar validações não executadas.

## 5. Defaults da skill (usados quando o usuário escolher "padrão")

| Parâmetro | Default |
|-----------|---------|
| Cores | Sem default fixo: vêm do briefing (seção 2.4, pergunta 7) |
| Tipografia | Bookerly Bold + Light se fornecidas; caso contrário, Georgia para headings e corpo, com fallbacks locais, informadas na confirmação |
| Logo | Assets fornecidos e escolhidos pelo usuário; na ausência deles e com decisão delegada, wordmark |
| Estilo | Editorial limpo: margens generosas, headlines grandes, bastante respiro |
| Tema inicial | Light |
| Tamanho | Definido pelo conteúdo |
| Saída | `./<slug-do-tema>-deck/` |

## 6. Identidade visual (aplicação do briefing)

### 6.1 Cores

- Use a paleta aprovada no briefing. O shell usa tokens de marca preenchidos com o briefing, sem uma paleta de marca fixa.
- Converta a paleta em custom properties: `--brand-1`, `--brand-2`, `--brand-3`, mais tokens derivados de superfície, texto (`--ink`), texto secundário (`--muted`), bordas e destaques, para light e dark.
- **Contraste mínimo (WCAG AA):** texto de corpo ≥ 4.5:1; headings grandes, ícones, bordas de controle e chips ≥ 3:1. Calcule, não estime.
- Cor da marca que falhe como texto sobre um fundo vira **cor de preenchimento ou acento** nesse fundo, e o texto usa `--ink` ou branco.
- No dark theme, ajuste a luminosidade das cores da marca para que continuem legíveis, sem trocar o matiz.

### 6.2 Tipografia

- Registre as fontes via `@font-face` por caminho relativo local (`assets/fonts/...`), com `font-display: swap` e uma pilha de fallback do sistema sempre declarada.
- Mapeie as fontes para `--font-heading` e `--font-body`. Se o usuário fornecer um único peso, crie a hierarquia com tamanho, espaçamento e caixa, e não com negrito sintético.
- Nunca use Google Fonts, CDN ou qualquer fonte remota.

### 6.3 Estilo

O estilo escolhido ajusta raio de borda, peso dos divisores, intensidade de cor nas superfícies e ritmo das animações. A base é sempre a mesma: margens generosas, headlines grandes, bastante respiro, composição limpa.

## 7. Logos e contraste (aplicar em ambos os temas)

- Escolha a logo pelo fundo real atrás dela, não pelo nome do tema global.
- Versão clara sobre fundos escuros, navy, imagem com overlay ou saturados, **inclusive capas em light theme**. Versão escura apenas sobre fundos claros/neutros com bom contraste.
- **Se o usuário fornecer só uma versão da logo:**
  - SVG monocromático: recolora via `fill="currentColor"` para gerar a variante oposta.
  - PNG monocromático: gere a variante com `filter: brightness(0) invert(1)` (ou o inverso).
  - Logo colorida/multicor: não aplique filtro. Use uma superfície de apoio local (plate/pill) atrás dela nos fundos em que ela perde contraste.
- Nunca distorça proporções. Defina altura máxima e respeite uma área de respiro ao redor da logo.
- Sem logo: use o nome do projeto/empresa como wordmark na fonte de heading.
- Sobre imagem ou gradiente misto, adicione uma superfície de apoio local sutil atrás da logo ou troque para a versão de maior contraste. (Erro já ocorrido: light theme com fundo azulado + logo azul = logo invisível.)
- Avalie o contraste **por bloco, não por slide**: capa, painéis de conteúdo, controles flutuantes, rodapé/paginação, texto sobre imagem ou superfície tingida.
- Bloco escuro em light theme → texto claro nesse bloco.
- Bloco claro em dark theme → texto escuro e ajuste dos destaques para o bloco continuar legível.
- Linhas decorativas, chips, divisores e botões visíveis em ambos os temas.
- Não presuma que um slide é legível só porque o fundo da página é legível.
- Sobre imagens/texturas, use overlay ou superfície atrás do texto quando necessário.

## 8. Conteúdo (copy)

- Reescreva texto bruto longo em copy pronta para apresentação, sem despejar parágrafos. Prefira poucas palavras fortes, headlines grandes, textos de apoio curtos e takeaways claros.
- Calibre a densidade pelo formato do briefing (projetado vs. leitura) e o tom pelo público.
- Use o contexto da empresa: fale a língua do setor dela, conecte o tema ao que ela faz e ao propósito dela, e evite copy genérica que serviria para qualquer empresa.
- Quando o conteúdo vier cru ou incompleto, infira um arco narrativo limpo, coerente com o objetivo declarado (informar, convencer, ensinar, reportar).
- Preserve o sentido factual ao condensar o conteúdo do usuário. Se o agente criou o conteúdo do zero, não invente números, nomes ou citações: use placeholders claros (ex.: `[KPI]`) e avise na entrega.
- Se o usuário pedir gráficos/diagramas/ilustrações mas não fornecer os ativos, crie placeholders estruturados com painéis, separadores e legendas de marca. Nunca puxe ativos remotos.

## 9. Canvas fixo, escala e anti-overflow (crítico)

O deck **nunca** mostra barra de rolagem, horizontal ou vertical, em janela normal ou em tela cheia. O layout não se reorganiza conforme a tela: ele é desenhado uma vez em 1920×1080 e escalado inteiro. Isso também garante que os textos fiquem sempre na mesma ordem e posição.

### 9.1 Estrutura obrigatória

```css
html, body { width: 100%; height: 100%; margin: 0; overflow: hidden; background: var(--stage-bg); }
.deck-viewport { position: fixed; inset: 0; overflow: hidden; background: var(--stage-bg); }
.deck-stage { position: absolute; left: 0; top: 0; width: 1920px; height: 1080px;
              overflow: hidden; transform-origin: 0 0; }
.slide { position: absolute; inset: 0; width: 1920px; height: 1080px; overflow: hidden;
         visibility: hidden; opacity: 0; pointer-events: none; }
.slide.active { visibility: visible; opacity: 1; pointer-events: auto; z-index: 1; }
```

```js
// Escala o canvas 1920×1080 para caber inteiro na janela, centralizado (letterbox).
const fit = () => {
  const s = Math.min(innerWidth / 1920, innerHeight / 1080);
  stage.style.transform =
    `translate(${(innerWidth - 1920 * s) / 2}px, ${(innerHeight - 1080 * s) / 2}px) scale(${s})`;
};
fit();
addEventListener('resize', fit); // também dispara ao entrar e sair da tela cheia
```

- `--stage-bg` é a cor das faixas que sobram fora do 16:9. Use a cor de fundo do tema para que as faixas pareçam parte do slide.
- Os controles do deck (seção 11) ficam **fora** do `.deck-stage`, em `position: fixed`, para não escalarem com o slide.

### 9.2 Regras de layout dentro do canvas

- Todos os tamanhos dentro do slide são em `px` do canvas 1920×1080 (ex.: título 96–130px, corpo 27–38px, padding do slide ~72px 100px 90px). **Não use** `vw`, `vh`, `dvh`, `clamp()` com unidades de viewport nem media queries para reorganizar o conteúdo: quem adapta à tela é a escala.
- Mantenha a ordem do HTML igual à ordem de leitura. Não reposicione blocos com `order`, `grid-auto-flow: dense` ou posicionamento absoluto que mude a sequência visual.
- Nenhum texto pode ser cortado, escondido ou empurrado para fora do slide. Quando um slide ficar cheio: reduza a copy, divida a ideia em outro slide ou mova o apoio para cards compactos. Não conte com o `overflow: hidden` para "arrumar" o layout.
- Salvaguardas: `min-width: 0` nos filhos de grid, `overflow-wrap: anywhere` para palavras longas, `text-wrap: balance` em headings, `table-layout: fixed` em tabelas.
- Fontes escolhidas pelo usuário podem ser mais largas que a Bookerly: revalide o encaixe com a fonte real do briefing.
- Imagens, SVG e vídeos: `max-width: 100%; max-height: 100%` dentro do bloco.

### 9.3 Impressão

Em `@media print`, use `@page { size: 1920px 1080px; margin: 0 }`, remova o `transform` do stage, mostre todos os slides (`position: relative; visibility: visible; opacity: 1`) com uma quebra de página por slide e esconda os controles.

## 10. Sistema de layouts

Ofereça pelo menos estas famílias e escolha a que melhor serve cada slide. Não cicle layouts mecanicamente: combine o layout ao conteúdo.

- **cover**: título hero, subtítulo, kicker opcional, branding forte, pouco ruído (sempre o slide 1).
- **split**: duas colunas equilibrando narrativa e evidência. Ideal para comparar/contrastar, antes/depois, texto + visual.
- **focus**: uma mensagem, tese, KPI, citação ou estatística centralizada com microcopy de apoio.
- **sidebar-left / sidebar-right**: trilho estreito para estrutura + área ampla para o conteúdo principal. Ideal para agenda, processo ou explicação estruturada. Espelhe os lados para variar o ritmo.
- **grid-panels**: de dois a quatro cards para comparações, pilares, capacidades ou matrizes.

Varie a composição mantendo um sistema reconhecível (mesmas fontes, cores, espaçamentos e componentes).

## 11. Navegação, transição e controles

- Setas ← → (e também `PageUp`/`PageDown`, `Espaço`, `Home`/`End`) navegam entre slides.

### Transição padrão (sem sobreposição de textos)

A troca de slide **não** faz crossfade entre dois slides. O slide anterior some na hora e só o conteúdo do novo slide entra com um fade curto, subindo levemente. Assim, dois textos nunca aparecem sobrepostos.

```css
/* O slide em si troca instantaneamente (sem transition em .slide). */
.slide .heading, .slide .content {
  opacity: 0; transform: translateY(12px);
  transition: opacity .28s ease, transform .28s ease;
}
.slide.active .heading, .slide.active .content { opacity: 1; transform: none; }
.slide.active .content { transition-delay: .06s; } /* o corpo entra logo depois do título */

@media (prefers-reduced-motion: reduce) {
  .slide .heading, .slide .content { transition: none; transform: none; }
}
```

```js
function showSlide(n) {
  n = Math.max(0, Math.min(n, slides.length - 1));
  slides.forEach((el, i) => {
    el.classList.toggle('active', i === n);
    el.setAttribute('aria-hidden', i !== n);
    el.inert = i !== n; // slide oculto não recebe foco nem cliques
  });
  counter.textContent = `${n + 1}/${slides.length}`;
  try { history.replaceState(null, '', `#${n + 1}`); } catch {}
}
```

- Cada slide tem um `.heading` (título e subtítulo) e um `.content` (corpo). Só esses dois blocos animam; fundo, logo e elementos decorativos ficam parados.
- Não adicione `transition` em `.slide` (opacity, transform ou visibility), não use deslizamento lateral nem animações longas: é isso que causa a sobreposição.
- Ao abrir, leia o `#n` da URL para retomar no slide certo.
- A roda do mouse pode avançar e voltar, com intervalo mínimo de ~650 ms entre trocas para não pular slides.

### Controles

- Canto superior direito, agrupados e minimalistas (fáceis de achar, sem competir com o conteúdo):
  1. Botão "Tela Cheia": dispara a Fullscreen API (papel do F11) e some enquanto o fullscreen está ativo (reaparece ao sair).
  2. Botão "Mudar Tema": alterna light/dark.
  3. Contador de páginas no formato `1/4`, atualizado corretamente.
- Os rótulos dos controles seguem o idioma do deck.

## 12. Requisitos técnicos

- Deck totalmente local: HTML, CSS, JS, fontes e imagens em caminhos relativos locais. Sem frameworks externos nem CDNs.
- Abre localmente sem servidor.
- Preserve seções semânticas de slide para o deck ser fácil de estender.
- Prefira CSS custom properties para controle de tema e de marca.

## 13. Checklist final (validar antes de entregar)

**Briefing**
- [ ] O briefing foi feito (ou pulado por pedido explícito do usuário) e o outline foi aprovado antes da geração.
- [ ] O contexto da empresa (o que faz e propósito) aparece na capa, no tom da copy e no fechamento.
- [ ] Cores, fontes, logo e estilo aplicados são exatamente os do briefing aprovado.
- [ ] `brief.md` salvo na pasta do deck.

**Deck**
- [ ] Abre localmente sem servidor; fontes e logos resolvem por caminho relativo local.
- [ ] Slide 1 é a capa.
- [ ] Há pelo menos 4 layouts distintos disponíveis no sistema.
- [ ] Sem barra de rolagem horizontal ou vertical, em janela normal, redimensionada e em tela cheia; o canvas 1920×1080 escala inteiro e centralizado.
- [ ] Nenhum texto cortado ou fora de ordem, com a fonte real escolhida.
- [ ] Transição: o slide anterior some na hora e só o título e o corpo do novo entram com fade curto; nenhum texto se sobrepõe.
- [ ] Contraste das cores da marca calculado (≥ 4.5:1 corpo, ≥ 3:1 headings/UI) nos dois temas.
- [ ] Contraste da logo verificado contra o bloco/fundo local, inclusive capas em light theme.
- [ ] Contador de páginas atualiza corretamente.
- [ ] Navegação por setas funciona nos dois sentidos.
- [ ] Fullscreen entra e sai de forma limpa e o botão "Tela Cheia" some durante o fullscreen.
- [ ] Troca de tema preserva a legibilidade de cada bloco.

**Entrega**
- [ ] Informe o caminho do deck, um resumo das escolhas aplicadas e qualquer placeholder (dados, imagens, KPIs) que o usuário precise completar.
