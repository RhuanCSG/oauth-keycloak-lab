# ADR-015 — Documentação de API: OpenAPI + Swagger (spec-first com validação)

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

As APIs de cada fase precisam ser documentadas de forma precisa e reproduzível. A documentação deve estar sincronizada com a implementação real, sem poluir o código com anotações.

## Decisão

Adotar a abordagem **spec-first com validação automatizada no CI**:

### Especificação
- O arquivo `openapi.yaml` é escrito **antes** da implementação das rotas — reforçando o fluxo de design antes do código
- Localização: `docs/openapi/openapi.yaml` em cada branch de fase
- Swagger UI servido localmente via `swagger-ui-express` para visualização durante o desenvolvimento

### Validação no CI
- O pipeline `ci.yml` executa validação de conformidade entre a especificação `openapi.yaml` e as rotas implementadas via **Dredd**
- Qualquer rota que não corresponda à spec bloqueia o pipeline

### Fluxo por fase
```
1. Escrever openapi.yaml (antes do código)
2. Implementar rotas (guiadas pela spec)
3. CI valida conformidade automaticamente
```

## Consequências

- O `openapi.yaml` funciona como contrato da API — escrito antes da implementação, alinhado com o papel de arquiteto adotado no projeto.
- O código não contém anotações de documentação — permanece limpo e focado na lógica.
- Desincronização entre spec e implementação é detectada automaticamente no pipeline.
- O Dredd é adicionado como dependência de desenvolvimento para validação de conformidade.
- O Swagger UI fica disponível localmente durante o desenvolvimento de cada fase.
