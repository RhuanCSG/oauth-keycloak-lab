# ADR-003 — Banco de dados: SQLite

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto precisa persistir usuários e tarefas. O banco de dados não é o foco do projeto — os fluxos OAuth são. A escolha deve minimizar fricção de setup sem comprometer a estrutura do código.

## Decisão

Usar **SQLite** como banco de dados em todas as fases que exigirem persistência.

O acesso ao banco será feito exclusivamente por meio de uma camada de abstração (ORM ou repositórios tipados), de forma que a troca de banco de dados não exija alterações fora dessa camada.

## Consequências

- Zero infraestrutura adicional: sem container de banco no Docker Compose.
- Dados persistidos em arquivo local (`.db`), incluído no `.gitignore`.
- A camada de abstração isola o restante do código de qualquer detalhe do SQLite.
- Trocar para PostgreSQL ou outro banco em um fork ou evolução futura requer apenas mudança na camada de dados.
- O foco do projeto permanece nos fluxos OAuth, não na infraestrutura de dados.
