# ADR-011 — Configuração do Keycloak

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O Keycloak precisa ser configurado com realm, clients, usuários e roles em cada fase que o utiliza (Fases 3 a 6). A configuração precisa ser reproduzível sem intervenção manual.

## Decisão

Usar **`realm-export.json` versionado + documentação explicativa no README de cada fase**.

- O `realm-export.json` é importado automaticamente pelo container do Keycloak na inicialização via variável de ambiente `KC_IMPORT`
- O arquivo fica em `keycloak/` na raiz de cada branch de fase
- O README de cada fase documenta em linguagem clara o que foi configurado: quais clients existem, quais roles, quais scopes, e por quê cada configuração é necessária

```
keycloak/
  realm-export.json    ← importado automaticamente no docker compose up
```

## Consequências

- `docker compose up` sobe o Keycloak já configurado, sem nenhuma etapa manual.
- O leitor entende a configuração pelo README sem precisar decifrar o JSON verboso do Keycloak.
- Mudanças na configuração do Keycloak são versionadas e revisáveis via git.
- A documentação do README precisa ser mantida sincronizada com o `realm-export.json`.
