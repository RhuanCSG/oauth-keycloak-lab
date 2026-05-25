# ADR-018 — Representação do papel de usuário na Fase 1

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

Na Fase 1, sem Keycloak, os papéis de usuário precisam ser gerenciados pela própria API. A abordagem deve ser simples — a Fase 1 existe para demonstrar o problema do auth acoplado, não para ser a solução definitiva.

## Decisão

Armazenar o papel do usuário em um campo **`papel`** na tabela `usuarios`, com os valores possíveis `usuario` e `admin`.

O papel é incluído no payload do JWT emitido pela API:

```json
{
  "sub": "uuid-do-usuario",
  "papel": "admin",
  "iat": 1234567890,
  "exp": 1234567890
}
```

O middleware de autorização lê o campo `papel` do token para decidir o nível de acesso.

## Consequências

- Sem tabela extra de papéis — schema mínimo e direto.
- O papel viaja no token, sem consulta adicional ao banco em cada requisição.
- Nas fases seguintes, essa responsabilidade migra para o Keycloak — o campo `papel` deixa de existir no banco e passa a vir como claim do token.
- A simplicidade desta abordagem reforça a narrativa: o próprio código demonstra o que precisa ser externalizado.
