# ADR-010 — Docker e Docker Compose

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto tem múltiplos serviços (Keycloak, resource-server, auth-service, frontend, worker). Precisamos definir o que roda em Docker e qual é o fluxo de desenvolvimento local.

## Decisão

Adotar a abordagem **híbrida**:

- **Docker Compose** orquestra a infraestrutura: Keycloak (e qualquer outro serviço de suporte futuro)
- **Serviços Node.js e frontend** rodam localmente durante o desenvolvimento (`npm run dev`)
- **Dockerfile** versionado em cada serviço Node.js e no frontend para documentar a containerização — quem quiser subir tudo via Docker tem o caminho disponível

### Fluxo de desenvolvimento
```
docker compose up        ← sobe Keycloak (e infraestrutura)
npm run dev              ← sobe cada serviço Node.js localmente
```

### Fluxo completo containerizado (opcional)
```
docker compose --profile full up   ← sobe tudo, incluindo os serviços Node.js
```

## Consequências

- Melhor DX no desenvolvimento: hot reload e debugging direto nos serviços Node.js.
- Cada serviço tem um `Dockerfile` que serve como documentação de containerização profissional.
- O `docker-compose.yml` de cada fase define perfis (`profiles`) para separar infraestrutura de aplicação.
- Zero dependência de configuração local além do Docker e do Node.js 24.16.0 LTS.
