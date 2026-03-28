# Spec 02 — Correcoes Imediatas: GA4 e baseurl

## Contexto

Acoes imediatas identificadas na spec 01.

## Mudancas

### 1. Migrar Google Analytics para GA4

- Atualizar `google_analytics` no `_config.yml` de `UA-26109148-1` para `G-D0Q14P3Y9E`
- Criar `_includes/google-analytics.html` sobrescrevendo o template do Minima 2.5
  (que usa o codigo UA antigo) com o snippet gtag do GA4, parametrizado via
  `{{ site.google_analytics }}`

### 2. Corrigir baseurl

- Alterar `baseurl` no `_config.yml` de `"/"` para `""` (string vazia), que e o
  valor correto para um site `usuario.github.io` sem subpasta
