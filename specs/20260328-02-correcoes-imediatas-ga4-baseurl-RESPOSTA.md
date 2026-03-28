# Resposta — Spec 02: Correcoes Imediatas: GA4 e baseurl

## Mudancas Aplicadas

### 1. Google Analytics GA4

- `_config.yml`: `google_analytics` atualizado de `UA-26109148-1` para `G-D0Q14P3Y9E`
- `_includes/google-analytics.html`: criado sobrescrevendo o template do Minima 2.5,
  usando o snippet gtag parametrizado via `{{ site.google_analytics }}`

O Minima 2.5 ja inclui a logica de injecao condicional (`{% if jekyll.environment == 'production' %}`
no `_includes/head.html` do tema), entao o script so sera emitido no deploy do
GitHub Pages, nao em `bundle exec jekyll serve` local.

### 2. baseurl

- `_config.yml`: `baseurl` alterado de `"/"` para `""` (string vazia)

Elimina o bug de barra dupla em URLs de assets (ex.: `//assets/main.css`) que
ocorria com a configuracao anterior.
