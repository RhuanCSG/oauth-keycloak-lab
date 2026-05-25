# ADR-005 — Gerenciador de pacotes: npm

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto tem múltiplos serviços, mas cada fase vive em uma branch independente — não é um monorepo que exige workspace sofisticado. O gerenciador de pacotes deve minimizar a fricção para qualquer desenvolvedor que clone o repositório.

## Decisão

Usar **npm** como gerenciador de pacotes em todos os serviços do projeto.

## Consequências

- Zero instalação adicional: npm é nativo do Node.js 24 LTS.
- Qualquer desenvolvedor clona o repositório e executa `npm install` sem configuração prévia.
- `package-lock.json` versionado em cada serviço para garantir builds reproduzíveis.
- `node_modules/` no `.gitignore` de cada serviço.
