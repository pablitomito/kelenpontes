# Kelen Pontes Estética Avançada — Landing Page

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Status](https://img.shields.io/badge/status-em%20produção-brightgreen)
![License](https://img.shields.io/badge/license-proprietário-lightgrey)

Landing page institucional de página única (*one-pager*) para **Kelen Pontes Estética Avançada**, clínica de estética avançada em Teresina, PI, especializada em emagrecimento, harmonização corporal e facial, depilação a laser e nail design.

Projeto desenvolvido como um artefato **HTML monolítico e autocontido**: zero dependências externas de build, zero requisições a CDNs de terceiros, sem necessidade de servidor de aplicação. Todo o CSS, JavaScript e os ativos de imagem (fotos de antes/depois, retrato, ícones) estão embutidos diretamente no documento.

## Sumário

- [Stack técnica](#stack-técnica)
- [Arquitetura](#arquitetura)
- [Funcionalidades](#funcionalidades)
- [Sistema de design](#sistema-de-design)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Ambiente de desenvolvimento](#ambiente-de-desenvolvimento)
- [Deploy](#deploy)
- [SEO e dados estruturados](#seo-e-dados-estruturados)
- [Acessibilidade](#acessibilidade)
- [Performance](#performance)
- [Roadmap / limitações conhecidas](#roadmap--limitações-conhecidas)
- [Licença](#licença)

## Stack técnica

| Camada | Tecnologia |
|---|---|
| Marcação | HTML5 semântico |
| Estilo | CSS3 (custom properties, Grid, Flexbox, `clip-path`, `aspect-ratio`) |
| Interatividade | JavaScript vanilla (ES6+), sem framework ou biblioteca |
| Tipografia | Google Fonts — Fraunces (display) e Jost (texto corrido) |
| Assets | Imagens rasterizadas, codificadas em Base64 e embutidas via `data:` URI |
| Build | Nenhum — arquivo único servido estaticamente |

Não há `package.json`, gerenciador de pacotes ou pipeline de build. O motivo é deliberado: o projeto prioriza portabilidade (um único arquivo `.html` pode ser aberto localmente, anexado a um e-mail ou implantado em qualquer host estático sem nenhuma etapa intermediária) em detrimento de modularização de código.

## Arquitetura

```
index.html
├── <head>
│   ├── metadados (title, description, viewport, Open Graph)
│   ├── JSON-LD (schema.org/BeautySalon)
│   ├── <link> — preconnect + stylesheet do Google Fonts
│   └── <style> — design tokens + todas as regras CSS do documento
└── <body>
    ├── <header> — navegação fixa
    ├── <section class="hero">
    ├── <section id="sobre">
    ├── <section id="servicos">
    ├── <section id="resultados"> — galeria de comparação antes/depois
    ├── <section id="depoimentos">
    ├── <section id="localizacao"> — endereço + iframe do Google Maps
    ├── <section class="cta-final">
    ├── <footer>
    └── <script> — lógica dos sliders de comparação
```

Não há roteamento client-side; a navegação entre seções é feita via âncoras (`#servicos`, `#resultados` etc.) com `scroll-behavior: smooth`.

## Funcionalidades

**Comparador de antes/depois (drag slider).** Cada resultado é renderizado como duas imagens sobrepostas (`.ba-before`/`.ba-after`), com a imagem "antes" recortada dinamicamente via `clip-path: inset()`, controlada por uma custom property CSS (`--pos`). O JavaScript atualiza essa variável em resposta a eventos de ponteiro (`pointerdown`/`pointermove`/`pointerup`), com suporte também a navegação por teclado (setas esquerda/direita) e atributos ARIA de slider (`role="slider"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax`).

**Tema adaptativo claro/escuro.** A paleta de cores é definida inteiramente por custom properties no seletor `:root`. Uma segunda definição de paleta é aplicada sob `@media (prefers-color-scheme: dark)`, herdando automaticamente a preferência do sistema operacional/navegador de quem visita. Um atributo `data-theme="dark"` (ou `"light"`) na tag `<html>` permite forçar um dos dois modos, sobrepondo a preferência do sistema.

**Integração com WhatsApp.** CTAs utilizam deep links no formato `https://wa.me/<DDI+DDD+número>?text=<mensagem pré-preenchida>`, abrindo a conversa já com uma mensagem sugerida.

**Mapa incorporado.** `<iframe>` do Google Maps via endpoint público (`maps.google.com/maps?q=...&output=embed`), sem necessidade de chave de API.

**Dados estruturados para SEO.** Bloco `<script type="application/ld+json">` com schema `BeautySalon` (schema.org), incluindo nome, endereço, telefone e perfil social — usado por mecanismos de busca para rich snippets.

## Sistema de design

### Paleta — modo claro (padrão)

| Token | Valor | Uso |
|---|---|---|
| `--bg` | `#FBF2E6` | Fundo geral da página |
| `--bg-alt` | `#F1DAC0` | Fundo alternativo (seções secundárias) |
| `--surface` | `#FFFDF9` | Cartões, blocos elevados |
| `--ink` | `#3A2A20` | Texto principal |
| `--ink-soft` | `#7A5D4E` | Texto secundário |
| `--accent` | `#A2553A` | Cor de destaque (CTAs, links) |
| `--gold` | `#B0813C` | Detalhes ornamentais |

### Paleta — modo escuro

| Token | Valor |
|---|---|
| `--bg` | `#211711` |
| `--surface` | `#2A1D16` |
| `--ink` | `#F3E6D8` |
| `--accent` | `#E08862` |
| `--gold` | `#DDB05C` |

### Tipografia

- **Display** (títulos): `Fraunces` — serifada, com eixos variáveis de peso e itálico, transmitindo o posicionamento editorial da marca.
- **Corpo**: `Jost` — geométrica sans-serif, alta legibilidade em blocos de texto longos.

## Estrutura do projeto

```
.
├── index.html      # aplicação completa (HTML + CSS + JS + assets em Base64)
└── README.md       # este arquivo
```

## Ambiente de desenvolvimento

Nenhuma instalação é necessária. Duas formas de rodar localmente:

```bash
# Opção 1 — servidor HTTP simples (Python)
python3 -m http.server 8000
# depois acesse http://localhost:8000/index.html

# Opção 2 — Live Server (extensão do VS Code)
# clique com o botão direito em index.html → "Open with Live Server"
```

Abrir o arquivo diretamente via protocolo `file://` (duplo clique) também funciona — não há chamadas a APIs externas além do carregamento das fontes do Google.

## Deploy

O projeto é compatível com qualquer plataforma de hospedagem de arquivos estáticos.

**Vercel (recomendado):**

```bash
git remote add origin <url-do-repositório>
git push -u origin master
```

Em seguida, importar o repositório em [vercel.com](https://vercel.com) → *Add New → Project*. Nenhuma configuração de build é necessária (*framework preset*: Other).

**Alternativa sem controle de versão:** [Netlify Drop](https://app.netlify.com/drop) — arrastar o `index.html` diretamente no navegador.

## SEO e dados estruturados

- Meta tags `description` e Open Graph (`og:title`, `og:description`, `og:type`) para pré-visualização em redes sociais.
- Marcação `BeautySalon` via JSON-LD para elegibilidade a rich results no Google.
- Hierarquia de headings semântica (`h1` único no hero, `h2` por seção).

## Acessibilidade

- Sliders de comparação implementam o padrão ARIA de `slider` (`role`, `aria-valuenow/min/max`, navegação por teclado).
- Respeita `prefers-reduced-motion`, desativando transições e scroll suave para usuários que configuraram essa preferência no sistema.
- Contraste de cores validado nas duas paletas (clara e escura) contra o texto sobreposto.
- Textos alternativos (`alt`) descritivos em todas as imagens.

## Performance

O arquivo único pesa aproximadamente **870 KB**, majoritariamente por conta das imagens embutidas em Base64 (fotos reais de antes/depois em alta resolução). Trade-off deliberado: elimina requisições HTTP adicionais (uma única requisição carrega o documento inteiro) em troca de um payload inicial maior. Para uma futura otimização, ver [Roadmap](#roadmap--limitações-conhecidas).

## Roadmap / limitações conhecidas

- [ ] Extrair imagens do Base64 inline para arquivos estáticos separados (`/assets`), permitindo cache de longo prazo pelo navegador e reduzindo o tamanho do documento HTML.
- [ ] Comprimir/otimizar imagens (WebP/AVIF) para reduzir o payload total.
- [ ] Formulário de agendamento nativo, como alternativa complementar ao redirecionamento para WhatsApp.
- [ ] Testes automatizados de regressão visual.

## Licença

Projeto proprietário, desenvolvido sob encomenda para Kelen Pontes Estética Avançada. Todos os direitos de conteúdo (fotografias, marca, textos) pertencem à cliente. Não licenciado para redistribuição ou reúso por terceiros.

---

**Contato do negócio:** [@kelenpontes.estetica](https://instagram.com/kelenpontes.estetica) · (86) 98193-8776 · Rua Professor Joca Vieira, 1387, Fátima, Teresina/PI
