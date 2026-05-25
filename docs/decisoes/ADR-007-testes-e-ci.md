# ADR-007 — Estratégia de testes e CI

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto é desenvolvido com TDD e precisa de uma estratégia de testes que cubra as camadas relevantes sem desviar o foco dos fluxos OAuth. Um pipeline de CI no GitHub Actions garante que cada fase seja verificável de forma automatizada.

## Decisão

### Framework
Usar **Vitest** como framework de testes em todos os serviços. Motivo: DX superior para TypeScript + ESM, watch mode eficiente para o ciclo TDD, e integração direta com o ecossistema de ferramentas adotado.

### O que testar

| Camada | Tipo | O que verificar |
|---|---|---|
| Lógica de domínio | Unitário | Validações puras, transições de status de tarefas |
| Repositórios | Integração | CRUD contra SQLite `:memory:` |
| Middleware de autenticação | Unitário | JWT válido passa; expirado → 401; escopo insuficiente → 403; token de serviço vs. usuário |
| Route handlers | Integração | Status codes, formato de resposta, regras de autorização por role (auth mockada) |
| Fluxos OAuth completos | E2E | Fluxo ponta a ponta com Keycloak via Docker (fases avançadas, pipeline separado) |

### O que não testar
Configuração do Keycloak, internals de repositórios, detalhes de implementação.

### Pipeline CI (GitHub Actions)
- Testes unitários e de integração rodam em todo Pull Request e push para qualquer branch `fase/*`.
- Testes E2E (com Keycloak via Docker) rodam em pipeline separado, acionado manualmente ou em merge para `main`.

## Consequências

- Todo código novo é precedido pelo teste (TDD).
- O middleware de autenticação — núcleo do projeto — tem cobertura explícita e documentada por testes.
- SQLite `:memory:` nos testes de repositório garante isolamento e velocidade sem infraestrutura adicional.
- O CI valida cada fase de forma automatizada antes de qualquer merge.
