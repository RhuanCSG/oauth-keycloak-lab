# Fase 1 — Auth Manual

**Branch:** `fase/1-auth-manual`

## Objetivo Pedagógico

Demonstrar os problemas de ter autenticação acoplada à API de negócio. Ao final desta fase, o leitor percebe que a API acumula responsabilidades que não são dela — gerenciamento de usuários, hashing de senhas, emissão de JWT, controle de papéis — e que qualquer outro serviço que precisasse de autenticação teria que replicar toda essa lógica.

## Serviços

Um único serviço: `api/` (monolito).

## Rotas

| Método | Rota | Papel | Descrição |
|---|---|---|---|
| `POST` | `/auth/registrar` | Público | Cadastra novo usuário |
| `POST` | `/auth/login` | Público | Autentica e retorna JWT |
| `GET` | `/tarefas` | `usuario`, `admin` | Lista tarefas do usuário autenticado |
| `POST` | `/tarefas` | `usuario`, `admin` | Cria tarefa para o usuário autenticado |
| `GET` | `/tarefas/:id` | `usuario`, `admin` | Busca tarefa própria por ID |
| `PATCH` | `/tarefas/:id` | `usuario`, `admin` | Atualiza tarefa própria |
| `DELETE` | `/tarefas/:id` | `usuario`, `admin` | Exclui tarefa própria |
| `GET` | `/admin/tarefas` | `admin` | Lista tarefas de todos os usuários |

## Schema do Banco

```sql
usuarios (
  id        TEXT PRIMARY KEY,       -- UUID v4
  nome      TEXT NOT NULL,
  email     TEXT NOT NULL UNIQUE,
  senha     TEXT NOT NULL,          -- hash:salt via node:crypto scrypt
  papel     TEXT NOT NULL           -- 'usuario' | 'admin'
)

tarefas (
  id          TEXT PRIMARY KEY,
  titulo      TEXT NOT NULL,
  descricao   TEXT,
  status      TEXT NOT NULL DEFAULT 'a_fazer',
  usuario_id  TEXT NOT NULL REFERENCES usuarios(id)
)
```

## JWT

- Biblioteca: `jose`
- Algoritmo: `HS256` (chave simétrica via variável de ambiente)
- Payload: `sub` (UUID do usuário) + `papel`

## Variáveis de Ambiente

```
PORT=3000
JWT_SECRET=your-secret-here
JWT_EXPIRATION=1h
DATABASE_PATH=./dados/banco.db
```

## Estrutura de Pastas

```
api/
  src/
    dominio/
      usuario.ts
      tarefa.ts
      senha.ts            ← gerarHashSenha, verificarSenha (node:crypto)
      erros.ts
    casos-de-uso/
      auth/
        registrar.ts
        login.ts
      tarefas/
        criar.ts
        listar.ts
        buscarPorId.ts
        atualizar.ts
        excluir.ts
    repositorios/
      IUsuarioRepositorio.ts
      ITarefaRepositorio.ts
      sqlite/
        cliente.ts
        migracoes.ts
        UsuarioRepositorio.ts
        TarefaRepositorio.ts
    rotas/
      auth.ts
      tarefas.ts
      admin.ts
    middlewares/
      autenticacao.ts
      autorizacao.ts
      erros.ts
    app.ts
    servidor.ts
  tests/
    dominio/
    casos-de-uso/
    repositorios/
    middlewares/
  docs/
    http/
    openapi/
  Dockerfile
  .env.example
  package.json
  tsconfig.json
```

## Cobertura de Testes (Vitest)

| Camada | O que verificar |
|---|---|
| Domínio | Hash e verificação de senha; validações de `Usuario` e `Tarefa` |
| Casos de uso | Registro com email duplicado; login com senha incorreta; CRUD com isolamento por `usuario_id` |
| Repositórios | Operações CRUD contra SQLite `:memory:` |
| Middlewares | JWT válido → passa; expirado → 401; papel incorreto → 403; ausente → 401 |

## O que o Leitor Vai Perceber

Ao final desta fase, fica evidente que a API carrega responsabilidades que não pertencem a ela:
- Gerenciar o cadastro e as senhas dos usuários
- Emitir e validar seus próprios tokens
- Controlar papéis de acesso internamente

Qualquer outro serviço no sistema que precisasse de autenticação teria que replicar toda essa lógica — o que é insustentável. Essa percepção motiva a Fase 2.
