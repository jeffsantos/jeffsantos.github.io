# Spec 01 — Avaliacao de Compatibilidade com Jekyll Atual

## Contexto

O site foi configurado inicialmente em 2022 e nao recebe manutencao ha algum tempo.
O objetivo e fazer um diagnostico do estado atual do projeto em relacao as versoes
e praticas recomendadas do Jekyll e do ecossistema GitHub Pages.

## Objetivo

Produzir um relatorio de compatibilidade identificando:

1. **Versoes desatualizadas** — `github-pages` gem (fixada em ~223), `minima` (~2.5),
   `jekyll-feed` (~0.12), `jekyll-responsive-image`, `tzinfo` (~1.2), `wdm` (~0.1.1)
2. **Google Analytics** — o ID `UA-26109148-1` e da geracao Universal Analytics,
   descontinuada pelo Google. Verificar se o site deveria migrar para GA4.
3. **`jekyll-responsive-image`** — avaliar se o plugin ainda e mantido e compativel
   com o ambiente atual do GitHub Pages.
4. **`baseurl` e `url`** — a configuracao atual tem `baseurl: "/"`, verificar se
   e o padrao correto para um site `usuario.github.io` (sem subpasta).
5. **Minima 2.5 vs 3.x** — verificar o que mudou e se ha ganhos relevantes para
   migrar ou se a versao atual ainda e adequada.
6. **GitHub Actions vs deploy classico** — o GitHub Pages atual recomenda deploy
   via GitHub Actions; verificar se o projeto se beneficiaria de migrar.

## Entrega

Um relatorio em `specs/20260328-01-avaliacao-compatibilidade-jekyll-RESPOSTA.md`
com:
- Situacao de cada item acima (ok / atencao / acao recomendada)
- Lista priorizada de acoes sugeridas, separando o que e simples atualizacao
  do que exige decisao ou esforco maior
- Nenhuma alteracao de codigo nesta spec — apenas diagnostico
