# Kelen Pontes Estética Avançada — Landing Page

Landing page (página única) para a clínica **Kelen Pontes Estética Avançada**, em Teresina, PI. Especialidades: emagrecimento, harmonização corporal e facial, depilação a laser e nail design.

- **Instagram:** [@kelenpontes.estetica](https://instagram.com/kelenpontes.estetica)
- **WhatsApp:** (86) 98193-8776
- **Endereço:** Rua Professor Joca Vieira, 1387 — Fátima, Teresina/PI

## Sobre o projeto

O site inteiro vive em um único arquivo: `index.html`. Não existe pasta de imagens, CSS ou JS separados — tudo (estilos, script e as fotos de antes/depois) está embutido dentro desse mesmo arquivo. Isso significa que:

- Não precisa de nenhuma instalação, dependência ou passo de build para rodar.
- Dá pra abrir o arquivo direto no navegador (duplo clique) que funciona.
- Pra fazer deploy, basta subir esse único arquivo — não tem `npm install`, não tem `package.json`, não tem nada pra compilar.

## Tecnologias

- HTML, CSS e JavaScript puros (sem framework, sem bibliotecas externas).
- Fontes: [Fraunces](https://fonts.google.com/specimen/Fraunces) (títulos) e [Jost](https://fonts.google.com/specimen/Jost) (texto corrido), carregadas via Google Fonts.
- Imagens embutidas como `base64` diretamente no HTML (por isso o arquivo é grande — quase 1MB).
- Slider de antes/depois construído com `clip-path` e eventos de ponteiro (`pointerdown`/`pointermove`), sem biblioteca nenhuma.
- Tema claro/escuro automático: o site se adapta sozinho à preferência do navegador de quem visita (`prefers-color-scheme`), usando variáveis CSS (veja mais em [Customização](#customização)).

## Como fazer o deploy

### Vercel (recomendado)

1. Crie um repositório no GitHub e suba esse arquivo (`git init`, `git add index.html README.md`, `git commit -m "primeira versão"`, depois crie o repositório no GitHub e faça `git push`).
2. Entre em [vercel.com](https://vercel.com), clique em **Add New → Project** e importe esse repositório.
3. Não precisa configurar nada (sem *build command*, sem *framework preset* — pode deixar como "Other" ou "Static"). Clique em **Deploy**.
4. A Vercel te dá um link público (tipo `seu-projeto.vercel.app`). Depois, em **Settings → Domains**, dá pra ligar um domínio próprio (ex: `kelenpontesestetica.com.br`).

Qualquer novo `git push` pra branch principal atualiza o site automaticamente.

### Alternativa sem Git: Netlify Drop

Se não quiser mexer com Git por enquanto, entre em [app.netlify.com/drop](https://app.netlify.com/drop) e arraste o `index.html` pra lá. Em poucos segundos você recebe um link público. (Pra atualizar depois, é só arrastar o arquivo de novo.)

## Customização

Praticamente tudo que pode precisar de ajuste está concentrado no bloco `:root{ ... }` no início do `<style>`, no topo do arquivo — são variáveis de cor (design tokens) usadas em todo o site:

```css
--bg: #FBF2E6;      /* fundo da página (modo claro) */
--accent: #A2553A;  /* cor de destaque (botões, links) */
--ink: #3A2A20;      /* cor do texto principal */
```

Mudando o valor de uma variável, ela muda em todos os lugares que a usam — não precisa caçar cor por cor no arquivo inteiro.

**Tema claro/escuro:** logo abaixo desse bloco existe um segundo grupo de variáveis dentro de `@media (prefers-color-scheme: dark)` — é a paleta escura, que entra automaticamente quando quem visita está com o navegador/celular no modo escuro. Pra forçar sempre um dos dois modos (ignorando a preferência de quem visita), veja a tag `<html lang="pt-BR">` bem no topo do arquivo e adicione `data-theme="dark"` ou `data-theme="light"` nela.

**Trocar uma foto:** como as imagens estão em `base64` (texto) embutido no HTML, não dá pra simplesmente arrastar um arquivo `.jpg` novo por cima. É preciso converter a nova imagem pra `base64` primeiro. Duas formas fáceis de fazer isso:

- Sites como [base64-image.de](https://www.base64-image.de/) — só subir a imagem e copiar o texto gerado.
- Depois, no HTML, procure a tag `<img src="data:image/jpeg;base64,...">` que você quer trocar, e substitua todo o texto entre aspas pelo novo código gerado.

**WhatsApp:** os links de agendamento seguem o formato `https://wa.me/5586981938776?text=...` — o número já vem preenchido; o texto depois do `?text=` é a mensagem que abre pronta na conversa.

**Google Maps:** o mapa embutido usa o endereço da clínica direto na URL do `iframe` (não precisa de chave de API). Pra trocar o endereço, basta editar o texto depois de `q=` na URL do `src` do `iframe`, na seção "Localização".


