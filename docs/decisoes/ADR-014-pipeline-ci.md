# ADR-014 — Pipeline de CI (GitHub Actions)

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto usa TDD (ADR-007) e precisa de validação automatizada a cada mudança. Testes unitários/integração e testes E2E têm custos de infraestrutura diferentes e devem ser separados.

## Decisão

Dois pipelines no GitHub Actions com responsabilidades distintas:

### Pipeline 1 — `ci.yml` (feedback rápido)

**Executa:** lint → formatação → testes unitários e de integração

**Gatilhos:**
- Push em qualquer branch `fase/*`
- Pull Request para qualquer branch
- Merge para `main`

### Pipeline 2 — `e2e.yml` (testes de ponta a ponta)

**Executa:** sobe Keycloak via Docker → aguarda healthcheck → testes E2E dos fluxos OAuth

**Gatilhos:**
- Merge para `main`
- Acionamento manual (`workflow_dispatch`)

### Matriz de execução

| Evento | `ci.yml` | `e2e.yml` |
|---|:---:|:---:|
| Push em `fase/*` | ✓ | — |
| Pull Request | ✓ | — |
| Merge para `main` | ✓ | ✓ |
| Acionamento manual | — | ✓ |

## Consequências

- Feedback imediato a cada push sem custo de infraestrutura do Keycloak.
- Testes E2E rodam nos momentos que importam: consolidação de fase na `main`.
- O pipeline `ci.yml` bloqueia merge em caso de falha de lint ou teste.
- O pipeline `e2e.yml` usa `services` do GitHub Actions para subir o Keycloak em container.
