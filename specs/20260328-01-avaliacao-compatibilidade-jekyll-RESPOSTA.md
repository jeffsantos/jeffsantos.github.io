# Resposta — Spec 01: Avaliacao de Compatibilidade com Jekyll Atual

## Resumo Executivo

O site tem tres problemas que requerem acao imediata (Google Analytics parou de
coletar dados, `baseurl` incorreto pode causar URLs quebradas, plugin nao suportado
pelo GitHub Pages) e varios itens de atualizacao de menor urgencia.

---

## Diagnostico por Item

### 1. Versoes dos Gems

| Gem | Versao atual | Versao atual no ecossistema | Situacao |
|---|---|---|---|
| `github-pages` | ~223 | 232 | **Atencao** — desatualizado 9 versoes |
| `minima` | ~2.5 | 2.5.2 (gem) | OK — a 2.5.x e a versao mais recente publicada no RubyGems |
| `jekyll-feed` | ~0.12 | 0.17.0 | **Atencao** — desatualizado, sem quebras, mas convem atualizar |
| `tzinfo` | ~1.2 | 2.0.6 | **Atencao** — constraint antiga; recomendado `>= 1, < 3` |
| `wdm` | ~0.1.1 | 0.2.0 (2024) | **Acao** — 0.1.1 e de 2015 e tem problemas com Ruby 3+; atualizar |

O `github-pages` bundla **Jekyll 3.10.0** (nao Jekyll 4). Isso e uma restricao
estrutural do pipeline classico do GitHub Pages — nao e um problema do projeto,
mas e importante saber que Jekyll 4 so e acessivel via GitHub Actions.

**Acao recomendada**: atualizar pins no `Gemfile` e rodar `bundle update`.

---

### 2. Google Analytics — CRITICO

O ID `UA-26109148-1` e da geracao **Universal Analytics**, que foi descontinuada
pelo Google em julho de 2023. O site **nao esta coletando dados** desde essa data.

Para migrar para GA4:
1. Criar uma propriedade GA4 no Google Analytics e obter o ID (`G-XXXXXXXXXX`)
2. Atualizar `google_analytics` no `_config.yml` para o novo ID
3. Sobrescrever `_includes/google-analytics.html` — o template do Minima 2.5
   usa o codigo antigo `analytics.js`; e necessario substituir pelo snippet gtag
   do GA4:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', '{{ site.google_analytics }}');
</script>
```

O Minima ja envolve esse include com `{% if jekyll.environment == 'production' %}`,
entao o script so dispara no GitHub Pages (nao em `jekyll serve` local).

**Acao recomendada**: migrar para GA4 — esta e a unica acao sem a qual o site
perde funcionalidade ja ativa.

---

### 3. jekyll-responsive-image — PROBLEMA ESTRUTURAL

Este plugin tem dois problemas graves:

- **Nao esta na lista de plugins permitidos pelo GitHub Pages** — o pipeline classico
  (build-from-branch) ignora silenciosamente plugins nao autorizados. O plugin
  provavelmente nunca funcionou no deploy.
- **Nao e mantido desde 2018** — repositorio com issues e PRs abertos sem resposta.

**Opcoes**:

a) **Remover o plugin** — se nenhuma pagina usa `{% responsive_image %}` ou
   `{% responsive_image_block %}`, basta remover do `Gemfile` e do `_config.yml`.
   Verificar os layouts e paginas antes.

b) **Substituir por `jekyll_picture_tag`** — alternativa ativa que gera elementos
   `<picture>` modernos, mas tambem nao esta na lista do GitHub Pages, exigindo
   GitHub Actions para o build.

c) **Migrar para GitHub Actions** — necessario se quiser usar qualquer plugin
   fora da lista. Abre caminho tambem para Jekyll 4 e outros recursos.

**Acao recomendada**: verificar se o plugin e efetivamente usado; se nao, remover.

---

### 4. baseurl — BUG POTENCIAL

A configuracao atual e `baseurl: "/"`. Para um site `usuario.github.io` (sem
subpasta), o correto e `baseurl: ""` (string vazia).

Com `baseurl: "/"`, Jekyll concatena a barra com os caminhos dos assets gerando
URLs como `//assets/main.css` (barra dupla), o que pode quebrar CSS, JS e links
em alguns contextos. Isso e um bug documentado no Jekyll (#6057).

**Acao recomendada**: alterar para `baseurl: ""` no `_config.yml`.

---

### 5. Minima 2.5 vs 3.x

Nao ha versao 3.x publicada no RubyGems — existe apenas no branch `main` do
repositorio do tema. Para usa-la seria necessario trocar `theme: minima` por
`remote_theme: jekyll/minima`, o que:
- Funciona no GitHub Pages via plugin `jekyll-remote-theme` (que esta na lista
  permitida)
- Traz suporte nativo a GA4, dark mode/skins, e nova estrutura de `site.author`
- Exige ajustes no `_config.yml` (estrutura de `social_links` mudou)

Nao ha urgencia nessa migracao. A versao 2.5.2 e a mais recente publicada e
ainda recebeu um patch em setembro de 2024.

**Acao recomendada**: avaliar futuramente, junto com eventual migracao para
GitHub Actions. Nao e prioritario.

---

### 6. GitHub Actions vs Deploy Classico

O deploy classico (build-from-branch) continua suportado e e adequado para este
site, desde que se mantenha dentro dos plugins autorizados pelo GitHub Pages.

A migracao para GitHub Actions valeria a pena se o projeto precisar de:
- Plugins nao autorizados (ex.: `jekyll-responsive-image`)
- Jekyll 4
- Minima 3.x via `remote_theme` com personalizacoes de build

Por ora, o deploy classico e suficiente.

**Acao recomendada**: manter deploy classico; reavaliar se remover o
`jekyll-responsive-image` nao resolver o problema dos plugins.

---

## Lista de Acoes Priorizadas

### Imediato (sem necessidade de decisao)

1. **Migrar Google Analytics para GA4** — criar propriedade GA4, atualizar ID no
   `_config.yml`, criar `_includes/google-analytics.html` com snippet gtag
2. **Corrigir `baseurl`** — alterar de `"/"` para `""` no `_config.yml`

### Curto prazo (requer verificacao rapida antes)

3. **Remover ou resolver `jekyll-responsive-image`** — verificar se e usado nos
   layouts/paginas; se nao, remover do `Gemfile` e `_config.yml`
4. **Atualizar pins no Gemfile** — `github-pages` para ~232, `jekyll-feed` sem
   pin fixo (ou ~0.17), `wdm` para ~0.2.0, `tzinfo` para `>= 1, < 3`

### Futuro (decisao estrategica)

5. **Avaliar migracao para Minima 3.x** — junto com eventual adocao de GitHub
   Actions, se houver interesse em dark mode, nova estrutura de config, GA4 nativo
