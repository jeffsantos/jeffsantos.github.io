# Resposta — Spec 03: Atualizacao de Gems e Remocao de jekyll-responsive-image

## Mudancas Aplicadas

### 1. Remocao de jekyll-responsive-image

Verificacao previa confirmou que nenhum layout ou pagina usa as tags do plugin
(`{% responsive_image %}` / `{% responsive_image_block %}`).

- `Gemfile`: removida linha `gem "jekyll-responsive-image"`
- `_config.yml`: removidos `- jekyll-responsive-image` de `plugins:` e o bloco
  `responsive_image:` inteiro (template e sizes)

### 2. Atualizacao dos pins no Gemfile

| Gem | De | Para |
|---|---|---|
| `github-pages` | ~223 | ~232 |
| `jekyll-feed` | ~0.12 | sem pin (usa versao do github-pages) |
| `tzinfo` | ~1.2 | >= 1, < 3 |
| `wdm` | ~0.1.1 | ~0.2.0 |

### 3. bundle update — pendente

O comando `bundle update` nao foi executado automaticamente pois o Ruby/Bundler
nao estava disponivel no PATH da sessao. Executar manualmente antes do proximo
deploy:

```bash
bundle update
```

Isso vai regenerar o `Gemfile.lock` com as versoes atualizadas e deve ser
commitado junto com o `Gemfile`.
