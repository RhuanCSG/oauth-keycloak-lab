# ADR-006 — Idioma do código: Português

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto é um portfólio e material de estudo voltado principalmente para desenvolvedores brasileiros. O idioma do código impacta a consistência e o público que consegue ler e entender o projeto com naturalidade.

## Decisão

Usar **português** em todo o código: nomes de variáveis, funções, interfaces, tipos, arquivos e comentários. Nomes de libs, APIs externas e convenções técnicas obrigatórias (nomes de campos de tokens OAuth, claims JWT, endpoints HTTP) permanecem em inglês por serem parte de especificações externas.

## Consequências

- Código alinhado com o público-alvo do projeto.
- Nomes de domínio (`tarefa`, `usuario`, `repositório`) convivem com termos técnicos obrigatórios em inglês (`access_token`, `client_id`, `Bearer`).
- A documentação e o código usam o mesmo idioma, mantendo consistência total para o leitor.
