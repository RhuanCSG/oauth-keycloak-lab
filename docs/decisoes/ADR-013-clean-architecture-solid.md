# ADR-013 — Clean Architecture e SOLID

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto é uma referência de qualidade. A organização do código precisa demonstrar boas práticas sem adicionar complexidade estrutural que desvie o foco dos fluxos OAuth.

## Decisão

Aplicar **princípios de Clean Architecture e SOLID de forma pragmática** — sem a cerimônia completa da Clean Architecture estrita.

### Estrutura de camadas

```
src/
  dominio/          ← entidades e regras de negócio puras (sem dependências externas)
  casos-de-uso/     ← lógica de aplicação; orquestra domínio e repositórios
  repositorios/     ← interfaces TypeScript + implementações com node:sqlite
  rotas/            ← handlers HTTP; delegam para casos de uso
  middlewares/      ← autenticação, autorização, erros
```

### Princípios SOLID aplicados

| Princípio | Aplicação |
|---|---|
| **S** — Responsabilidade única | Cada módulo tem uma razão para mudar |
| **O** — Aberto/fechado | Middlewares extensíveis sem modificação |
| **L** — Substituição de Liskov | Implementações de repositório respeitam a interface |
| **I** — Segregação de interfaces | Interfaces de repositório específicas por entidade |
| **D** — Inversão de dependência | Casos de uso dependem de interfaces, não de implementações concretas |

### O que não será feito
- Interfaces para tudo (apenas onde a inversão agrega valor real: repositórios)
- Camadas de mapeamento entre DTOs em todas as fronteiras
- Estrutura de pastas com `adapters/`, `presenters/`, `gateways/` separados

## Consequências

- Código organizado em camadas com direção de dependência clara: rotas → casos de uso → repositórios → domínio.
- Repositórios são testáveis de forma isolada via SQLite `:memory:`.
- Casos de uso são testáveis sem framework HTTP.
- A estrutura é consistente entre todas as fases do projeto.
