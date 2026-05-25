# ADR-002 — Sistema de módulos: ESM

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

Node.js suporta dois sistemas de módulos: CommonJS (legado) e ESM (padrão moderno). A escolha impacta a compatibilidade com libs, a configuração do projeto e o alinhamento com o ecossistema atual.

A lib `jose`, usada para validação de JWT, é ESM-native. O Node.js 22 LTS tem suporte robusto a ESM sem flags experimentais.

## Decisão

Usar **ESM** (`import`/`export`) em todos os pacotes do projeto, com `"type": "module"` no `package.json` de cada serviço.

## Consequências

- Todos os arquivos `.ts` usam sintaxe `import`/`export`.
- Imports de arquivos locais incluem a extensão `.js` (comportamento do TypeScript com ESM).
- Compatibilidade com `jose` e demais libs ESM-first garantida sem workarounds.
- Pacotes legados CommonJS-only devem ser evitados; se necessários, exigem avaliação caso a caso.
