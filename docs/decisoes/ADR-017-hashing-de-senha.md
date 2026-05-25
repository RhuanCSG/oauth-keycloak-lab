# ADR-017 — Hashing de senha: node:crypto com scrypt

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

Na Fase 1, a API gerencia senhas de usuários diretamente. Precisamos de uma função de hash segura sem adicionar dependências externas.

## Decisão

Usar o módulo nativo **`node:crypto`** com o algoritmo **`scrypt`** para hashing e verificação de senhas.

A lógica de hashing é encapsulada em um módulo dedicado dentro de `dominio/`, expondo apenas duas funções:

```ts
gerarHashSenha(senha: string): Promise<string>
verificarSenha(senha: string, hash: string): Promise<boolean>
```

O hash armazenado inclui o salt concatenado — sem campo separado no banco.

## Consequências

- Zero dependência externa para hashing de senhas.
- `scrypt` é recomendado pelo OWASP para armazenamento de senhas.
- O módulo de hashing é testável de forma isolada via Vitest.
- Alinhado com o princípio de preferir recursos nativos do Node.js 24.16.0 LTS.
