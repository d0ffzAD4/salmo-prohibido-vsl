# Prompt para Claude Code — Quiz "Prueba de la Prosperidad"

Cole esse prompt inteiro no Claude Code dentro da pasta vazia onde você quer o projeto.

---

## CONTEXTO

Estou migrando uma página de quiz/funil que estava hospedada na plataforma Inlead para um site próprio. O quiz é em espanhol, mobile-first, tem 14 telas sequenciais, e termina em um CTA que leva para um checkout/VSL.

Já tenho um HTML funcional pronto (`index.html` em anexo) que replica o quiz original. Sua tarefa é melhorar esse arquivo e preparar tudo para produção.

## OBJETIVO

Entregar um projeto pronto para deploy em **Vercel/Netlify/Cloudflare Pages**, leve, sem framework, sem build step. Apenas HTML + CSS + JS vanilla.

## TAREFAS

### 1. Estrutura final do projeto
```
/
├── index.html
├── /images
│   ├── hombre.png
│   ├── mujer.png
│   ├── mercado.png      (tela 6 — pessoa do mercado com frequências)
│   ├── bloqueo.png      (tela 8 — pessoa orando em ambiente rústico)
│   ├── palabras.png     (tela 11 — pessoa com luz/palavras nas mãos)
│   └── torah.png        (tela 13 — pergaminho/torá com mão escrevendo)
├── /og
│   └── og-image.jpg     (1200x630, para compartilhamento social)
└── README.md
```

### 2. Ajustes no HTML existente
- **Meta tags**: adicionar Open Graph, Twitter Card, favicon, theme-color (`#3a1f0a`), description em espanhol.
- **SEO básico**: `<title>`, `<meta name="description">`, `lang="es"` (já tem).
- **Performance**:
  - Preload da fonte Inter (já tem preconnect, adicionar preload do woff2).
  - `loading="lazy"` em imagens fora da viewport inicial.
  - Inline crítico do CSS da tela 1 (acima da dobra) — opcional, só se valer a pena.
- **Acessibilidade**:
  - `aria-label` no botão voltar (já tem).
  - `aria-live="polite"` no container da progress bar.
  - `role="progressbar"` com `aria-valuenow` dinâmico.
  - Focus visible em todos os botões.

### 3. Tracking / Pixels
Adicionar no `<head>` (deixar placeholders para eu preencher):
- Meta Pixel (Facebook) — placeholder `PIXEL_ID`.
- Disparar evento `Lead` quando o usuário clicar no CTA final.
- Disparar eventos customizados por tela: `QuizStep_1`, `QuizStep_2`, etc. — útil pra ver onde tem drop-off.
- Deixar bloco comentado para Google Tag Manager (caso eu queira usar depois).

### 4. Envio das respostas
Hoje as respostas só ficam em `console.log`. Quero duas opções:
- **A)** Enviar via `fetch` POST para um webhook (placeholder `WEBHOOK_URL`).
- **B)** Passar todas as respostas como query params na URL do CTA final (pra eu capturar no checkout/VSL).

Implementar as duas, controladas por uma flag no topo do script:
```js
const CONFIG = {
  WEBHOOK_URL: '', // se vazio, não envia
  FINAL_URL: '',   // URL do checkout/VSL
  PASS_ANSWERS_AS_QUERY: true,
  PIXEL_ID: '',
};
```

### 5. README.md
Escreva um README curto cobrindo:
- Como configurar (editar `CONFIG` no topo do `<script>`).
- Como trocar as imagens (lista das 6 imagens necessárias com descrição do que cada uma é).
- Como adicionar o Pixel.
- Como fazer deploy (Vercel drag-and-drop, Netlify, Cloudflare Pages).
- Estrutura do objeto `answers` (pra eu saber o que vou receber no webhook).

### 6. Não mexer
- **Não mude a paleta de cores** (marrom `#3a1f0a` + dourado `#c89020`).
- **Não mude o copy em espanhol** — está validado.
- **Não troque por React/Vue/Next** — vanilla é proposital, quero zero dependência de build.
- **Não mude a ordem das telas** — é uma sequência de funil pensada.

## DEPOIS DE TERMINAR

Rode `python3 -m http.server 8000` na raiz do projeto, abra `http://localhost:8000`, clique em todas as 14 telas até o final, e me confirme que:
1. Progress bar avança suavemente.
2. Botão voltar funciona em todas as telas (exceto a 1ª).
3. Multi-select da tela 12 permite marcar/desmarcar várias.
4. Console mostra o objeto `answers` ao clicar no CTA final.
5. Sem erros no console.

## OBSERVAÇÃO IMPORTANTE

As 6 imagens em `/images/` são placeholders no HTML atual (aparece o nome do arquivo). Você **não precisa criar as imagens** — eu vou subir manualmente depois. Só garanta que os caminhos no CSS (`url('images/xxx.png')`) estão corretos e que o fallback (texto do nome do arquivo) está visível enquanto a imagem não existir.
