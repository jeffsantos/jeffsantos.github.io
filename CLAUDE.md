# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal academic website for Prof. Jefferson Santos, hosted on GitHub Pages at `jeffsantos.github.io`. Built with Jekyll using the Minima theme.

## Development Commands

```bash
# Install dependencies
bundle install

# Start local development server (with live reload)
bundle exec jekyll serve

# Build static site to _site/
bundle exec jekyll build
```

There is no test suite. Deployment is automatic — pushes to `main` trigger GitHub Pages to rebuild and publish the site.

## Architecture

### Content structure

- `index.markdown`, `about.markdown`, `blog.markdown`, `contact.markdown`, `research.markdown`, `teaching.markdown` — top-level pages (front matter sets layout and title)
- `_posts/` — blog posts in `YYYY-MM-DD-title.markdown` format
- `_layouts/home.html` and `_layouts/blog.html` — custom layouts that extend Minima's base layouts
- `assets/` — static files (images, etc.)

### Configuration

`_config.yml` is the central config: site metadata, Google Analytics ID (`UA-26109148-1`), theme settings, and responsive image sizes (320px, 480px, 800px via `jekyll-responsive-image`).

### Theme customization

The site uses Minima ~2.5. Custom layouts in `_layouts/` override or extend Minima's defaults. To override other theme files (CSS, includes), create matching paths locally — Jekyll prefers local files over theme files.

### Deployment

GitHub Pages builds from the `main` branch using the `github-pages` gem (pin ~223 in Gemfile), which constrains available plugins to GitHub's allowlist. Jekyll plugins must be on the [GitHub Pages dependency list](https://pages.github.com/versions/).

## Workflow com Specs

Especificacoes ficam em `specs/` com formato `yyyymmdd-99-titulo-curto.md`. O arquivo `specs/backlog.md` e o indice vivo das specs — atualizar sempre que uma spec for criada ou concluida.

O fluxo completo:

1. Usuario descreve informalmente o que quer
2. Claude cria a spec em `specs/` e aguarda aprovacao
3. Usuario aprova (ou ajusta) a spec
4. Claude implementa e cria arquivo `specs/yyyymmdd-99-titulo-curto-RESPOSTA.md` com analise e decisoes
5. Claude comita spec, resposta e codigo alterado juntos; marca spec como `[x]` no `backlog.md`

### Ajustes a Specs

Ajustes sao **acrescimos** a specs existentes, nao arquivos separados:

1. Na spec original: adicionar secao `## Ajustes` no fim, com subsecoes numeradas (`### Ajuste 1: Titulo`, `### Ajuste 2: Titulo`, etc.)
2. Na resposta: adicionar secao `## Respostas aos Ajustes` com subsecoes correspondentes
3. Comitar spec atualizada, resposta atualizada e codigo alterado juntos

**Nunca criar arquivos separados** como `spec-01a-ajuste.md`. Ajustes vivem no mesmo arquivo da spec original.

## Commits

Conventional Commits em **portugues**, max 80 caracteres, imperativo:

```
feat(layout): adiciona cabecalho responsivo
fix(config): corrige base URL para GitHub Pages
docs(specs): documenta redesign da pagina inicial
```

Atribuicao para commits assistidos:
```
Gerado por Claude (https://claude.ai/code) sob a supervisao humana de <user.name> (<user.email>)
```
