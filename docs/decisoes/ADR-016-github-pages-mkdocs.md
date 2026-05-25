# ADR-016 — Documentação no GitHub Pages: MkDocs + Material

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto precisa de uma documentação navegável e publicada, que consolide a narrativa das fases, os ADRs, o glossário OAuth/OIDC e a referência das APIs. O GitHub Pages é o destino natural para um projeto de portfólio.

## Decisão

Usar **MkDocs com o tema Material** para gerar e publicar a documentação no GitHub Pages.

### O que é publicado
- Visão geral do projeto e narrativa das 6 fases
- ADRs navegáveis
- Glossário OAuth/OIDC
- Diagramas de sequência dos fluxos (via suporte nativo a Mermaid do tema Material)
- Referência das APIs (Swagger UI embutido via plugin)

### Estrutura
```
docs/
  index.md              ← visão geral
  fases/                ← narrativa de cada fase
  decisoes/             ← ADRs (os mesmos arquivos já existentes)
  glossario.md
  openapi/              ← spec OpenAPI de cada fase
mkdocs.yml              ← configuração do MkDocs
```

### Deploy
- Pipeline dedicado `docs.yml` no GitHub Actions
- Disparado em push para `main` (onde vivem os ADRs e a documentação geral)
- Usa `mkdocs gh-deploy` para publicar no branch `gh-pages`

## Consequências

- READMEs e ADRs em Markdown são aproveitados diretamente — mínima reescrita.
- Python é adicionado como dependência de build exclusivamente no pipeline de documentação.
- O tema Material fornece suporte nativo a Mermaid — os diagramas de sequência dos fluxos OAuth são renderizados automaticamente.
- Um terceiro pipeline (`docs.yml`) é adicionado ao projeto, separado do `ci.yml` e do `e2e.yml`.
