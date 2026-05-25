# Modelo de Faseamento

## Visão Geral

O projeto é estruturado em 6 fases, cada uma em uma branch independente e completa. A progressão conta uma história coerente: cada fase expõe um problema que a próxima resolve.

Qualquer branch pode ser clonada e executada de forma isolada — sem depender de outra fase.

## Fases

| Branch | Nome | Narrativa |
|---|---|---|
| `fase/1-auth-manual` | Auth Manual | API monolítica com auth acoplada — expõe o problema |
| `fase/2-auth-service` | Auth Service | Extração da auth para serviço dedicado — solução parcial |
| `fase/3-keycloak-setup` | Keycloak Setup | Substituição pelo Keycloak como IdP — solução real |
| `fase/4-authorization-code-pkce` | Authorization Code + PKCE | Fluxo de usuário autenticando via SPA |
| `fase/5-client-credentials` | Client Credentials | Fluxo M2M sem contexto de usuário |
| `fase/6-refresh-token` | Refresh Token | Gestão de sessão e logout federado |

## Serviços por Fase

| Fase | Keycloak | api / resource-server | auth-service | frontend | worker |
|---|:---:|:---:|:---:|:---:|:---:|
| 1 | — | ✓ (monolito) | — | — | — |
| 2 | — | ✓ | ✓ | — | — |
| 3 | ✓ | ✓ | — | — | — |
| 4 | ✓ | ✓ | — | ✓ | — |
| 5 | ✓ | ✓ | — | ✓ | ✓ |
| 6 | ✓ | ✓ | — | ✓ | ✓ |

## Regras do Modelo

- Cada branch tem seu próprio `docker-compose.yml`, `package.json`, `README.md`, testes e coleção HTTP.
- A `main` contém apenas documentação geral, ADRs e o `CLAUDE.md` — sem código de aplicação.
- Ao trabalhar em uma fase, nunca assumir que arquivos de outra fase existem.
- O leitor pode fazer checkout de qualquer fase e ter o ambiente funcionando com `docker compose up` + `npm run dev`.
