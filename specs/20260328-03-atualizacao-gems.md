# Spec 03 — Atualizacao de Gems e Remocao de jekyll-responsive-image

## Contexto

Acoes de curto prazo identificadas na spec 01. Verificacao previa confirmou que
nenhum layout ou pagina usa as tags `{% responsive_image %}` ou
`{% responsive_image_block %}`.

## Mudancas

### 1. Remover jekyll-responsive-image

O plugin nao e usado no conteudo do site, nao esta na lista de plugins permitidos
pelo GitHub Pages (nunca funcionou no deploy) e nao recebe manutencao desde 2018.

- Remover `gem "jekyll-responsive-image"` do `Gemfile`
- Remover o plugin de `plugins:` no `_config.yml`
- Remover o bloco `responsive_image:` no `_config.yml`

### 2. Atualizar pins dos gems no Gemfile

| Gem | De | Para |
|---|---|---|
| `github-pages` | ~223 | ~232 |
| `jekyll-feed` | ~0.12 | sem pin fixo (usa a versao do github-pages) |
| `tzinfo` | ~1.2 | >= 1, < 3 |
| `wdm` | ~0.1.1 | ~0.2.0 |

Apos as alteracoes no `Gemfile`, rodar `bundle update` para regenerar o
`Gemfile.lock` com as versoes atualizadas.
