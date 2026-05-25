# ADR-001 — Linguagem: TypeScript

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto é uma referência de qualidade para outros desenvolvedores. A escolha entre TypeScript e JavaScript impacta a legibilidade, a segurança de tipos entre camadas e o quanto o projeto representa um ambiente de produção real.

Node.js 22 LTS suporta execução nativa de TypeScript via `--experimental-strip-types`, reduzindo o atrito de configuração em desenvolvimento.

## Decisão

Usar **TypeScript** em todos os serviços Node.js do projeto.

## Consequências

- Cada serviço terá seu próprio `tsconfig.json` alinhado às recomendações da documentação oficial do TypeScript.
- Contratos entre camadas (ex: payload de JWT, resposta do token endpoint) serão expressos como tipos explícitos.
- A configuração de execução aproveitará o suporte nativo do Node.js 22 LTS onde aplicável.
- O projeto representa melhor o que um desenvolvedor encontrará em ambientes de produção.
