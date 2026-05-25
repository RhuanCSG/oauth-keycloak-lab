# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Propósito do Projeto

Projeto de portfólio e estudo que demonstra a evolução de uma aplicação desde autenticação manual até fluxos OAuth 2.0 e OIDC completos com Keycloak. A narrativa é intencional: cada fase expõe um problema real que a próxima fase resolve.

Domínio de aplicação: **Task Manager** — gerenciamento de tarefas com roles de usuário comum, admin e worker service.

## Princípios Inegociáveis

- **Sem boilerplate copiado.** Cada arquivo começa do zero, guiado pela documentação oficial da tecnologia em questão.
- **Sem rotas simuladas em arquivo único.** A estrutura de pastas reflete responsabilidades reais desde a Fase 1.
- **Qualidade de referência.** Nomes de variáveis, funções e arquivos são a documentação primária.
- **Documentação oficial como fonte da verdade.** RFC 6749, OpenID Connect Core spec, docs do Keycloak, jose, oidc-client-ts.
- **TDD.** Todo código novo é precedido pelo teste.
- **Menos dependências externas.** Preferir recursos nativos do Node.js antes de adicionar pacotes.

## Decisões Técnicas (ver ADRs em `docs/decisoes/`)

| Decisão | Escolha |
|---|---|
| Linguagem | TypeScript |
| Sistema de módulos | ESM (`import`/`export`, `"type": "module"`) |
| Runtime | Node.js 24.16.0 LTS |
| Banco de dados | SQLite via `node:sqlite` (nativo do Node.js 24) |
| Acesso a dados | Repository Pattern com interfaces TypeScript — sem ORM |
| Gerenciador de pacotes | npm |
| Idioma do código | Português (exceto termos técnicos obrigatórios de specs externas) |
| Testes | Vitest |
| Linting / Formatação | ESLint + Prettier (`eslint-config-prettier`) |
| Cliente HTTP | Arquivos `.http` (versionados) + Postman collection |
| Identity Provider | Keycloak via Docker |
| Backend | Node.js + Express |
| Frontend | React + Vite |
| Validação JWT | jose |
| Biblioteca OIDC (frontend) | oidc-client-ts |

## Regras de Negócio

### Atributos de tarefa
`id`, `titulo` (obrigatório), `descricao` (opcional), `status` (`a_fazer` · `fazendo` · `feito`), `usuario_id`

### Papéis e operações
- **Usuário comum:** CRUD nas próprias tarefas
- **Admin:** CRUD em tarefas de todos os usuários
- **Worker:** leitura de todas as tarefas + relatório de status agregado (via Client Credentials)

### Regras de isolamento
- Usuário nunca acessa tarefas de outro usuário, mesmo conhecendo o ID
- Admin identificado por role no token JWT
- Worker identificado pela ausência de `sub` de usuário (token de serviço)

## Modelo de Branches

Cada branch `fase/N-*` é um snapshot completo e independente — tem seu próprio `docker-compose.yml`, `package.json`, `README.md`, testes e coleção HTTP. Nenhuma branch depende de outra para funcionar.

```
main                              ← documentação, ADRs, CLAUDE.md (sem código de aplicação)
fase/1-auth-manual                ← API monolítica com auth acoplada
fase/2-auth-service               ← auth-service + resource-server
fase/3-keycloak-setup             ← Keycloak + resource-server
fase/4-authorization-code-pkce    ← + frontend React
fase/5-client-credentials         ← + worker service
fase/6-refresh-token              ← gestão de sessão e logout federado
```

Ao trabalhar em uma fase, nunca assumir que arquivos de outra fase existem.

## Arquitetura de Código

Estrutura de camadas por serviço (Clean Architecture pragmática — ADR-013):

```
<servico>/
  src/
    dominio/        ← entidades e regras puras, sem dependências externas
    casos-de-uso/   ← lógica de aplicação; orquestra domínio e repositórios
    repositorios/   ← interfaces TypeScript + implementações com node:sqlite
    rotas/          ← handlers HTTP; delegam para casos de uso
    middlewares/    ← autenticação, autorização, erros
  tests/
  Dockerfile
  .env.example
  package.json
  tsconfig.json
docs/                          ← apenas na branch main
  decisoes/                   ← ADRs: decisões arquiteturais e técnicas
  fases/                       ← planejamento detalhado de cada fase
    modelo-de-faseamento.md
    fase-1.md
    fase-2.md  ...
  negocio/                     ← domínio, regras de negócio, papéis
    dominio.md
    regras.md
  http/                        ← arquivos .http + postman_collection.json (por fase)
  openapi/                     ← openapi.yaml de cada fase (escrito antes das rotas)
```

Direção de dependência: `rotas → casos-de-uso → repositórios → domínio`. Casos de uso dependem de interfaces de repositório, não de implementações concretas.

## Infraestrutura e Ambiente

- **Docker Compose** orquestra Keycloak. Serviços Node.js rodam localmente em desenvolvimento.
- Cada serviço tem um `Dockerfile` para containerização completa opcional (`--profile full`).
- **Keycloak** configurado via `realm-export.json` importado automaticamente na inicialização. O README de cada fase documenta o que foi configurado e por quê.
- Twelve-Factor App aplicado nos fatores relevantes: config via env vars, serviços stateless, logs para stdout.

## Pipelines (GitHub Actions)

| Pipeline | Gatilho | O que executa |
|---|---|---|
| `ci.yml` | Push `fase/*`, PRs, merge `main` | Lint → Prettier → Vitest (unit + integração) → validação OpenAPI (Dredd) |
| `e2e.yml` | Merge `main`, manual | Keycloak via Docker → testes E2E dos fluxos OAuth |
| `docs.yml` | Push `main` | MkDocs build → deploy GitHub Pages |

## Documentação de API

- Abordagem **spec-first**: `openapi.yaml` é escrito antes da implementação das rotas.
- Localização: `docs/openapi/openapi.yaml` em cada branch de fase.
- Swagger UI disponível localmente via `swagger-ui-express`.
- CI valida conformidade entre spec e implementação via Dredd.

## GitHub Pages

- Gerado com **MkDocs + tema Material**.
- Publica: visão geral, narrativa das fases, ADRs, glossário OAuth/OIDC, diagramas Mermaid, referência das APIs.
- Configuração em `mkdocs.yml` na raiz da `main`.

## Convenções

- JWT validado com `jose` — nunca decodificar sem verificar assinatura.
- Secrets em `.env` (no `.gitignore`), espelhados em `.env.example` com placeholders descritivos.
- Commits em português, atômicos por funcionalidade.
- `openapi.yaml` escrito antes de qualquer rota — spec define o contrato, código implementa.

## Regras de Comportamento do Agente

### Git
Nunca executar qualquer operação git sem pedido explícito do usuário.

### Skills
Nunca invocar skills automaticamente. Só invocar quando o usuário pedir explicitamente.

### Papel de Arquiteto
Antes de qualquer implementação: mapear o entendimento, identificar requisitos ausentes, perguntar ao usuário. Só escrever código após confirmação explícita. Para cada decisão: apresentar prós e contras e dar uma recomendação fundamentada.

### Dados Sensíveis
Nunca escrever secrets em arquivos. Se detectar dado sensível em um arquivo prestes a ser criado ou editado, bloquear e avisar antes de continuar.
