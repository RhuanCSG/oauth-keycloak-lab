# ADR-008 — Linting e formatação: ESLint + Prettier

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto precisa de consistência de estilo e detecção de problemas em tempo de desenvolvimento. A escolha das ferramentas impacta a configuração inicial e a experiência de desenvolvimento.

## Decisão

Usar **ESLint** para linting e **Prettier** para formatação em todos os serviços do projeto.

- ESLint com `@typescript-eslint` para regras específicas de TypeScript
- Prettier para formatação consistente, integrado ao ESLint via `eslint-config-prettier` para eliminar conflitos entre as duas ferramentas
- Configurações compartilhadas na raiz de cada branch de fase

## Consequências

- Dois pacotes de tooling (mais suas dependências de plugin).
- `eslint-config-prettier` é obrigatório para desativar regras do ESLint que conflitam com o Prettier.
- O pipeline de CI valida lint e formatação antes de rodar os testes.
- Configuração versionada (`.eslintrc` e `.prettierrc`) garante consistência entre ambientes de desenvolvimento.
