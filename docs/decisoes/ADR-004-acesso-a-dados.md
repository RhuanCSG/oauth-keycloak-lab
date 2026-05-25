# ADR-004 — Acesso a dados: node:sqlite + Repository Pattern

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto utiliza SQLite como banco de dados (ADR-003). Precisamos definir como o código acessa o banco, com o princípio de minimizar dependências externas e manter a abstração que permite trocar o banco sem impacto no restante da aplicação.

O Node.js 24 LTS estabilizou o módulo nativo `node:sqlite`, eliminando a necessidade de qualquer pacote externo para acesso ao SQLite.

## Decisão

Usar o módulo nativo **`node:sqlite`** do Node.js 24 LTS como driver de banco de dados, sem ORM.

O acesso ao banco será encapsulado em **repositórios tipados**: cada entidade tem seu próprio repositório com uma interface TypeScript definida. O restante da aplicação depende apenas da interface, nunca do driver diretamente.

```
src/
  repositorios/
    IUsuarioRepositorio.ts
    ITarefaRepositorio.ts
    sqlite/
      cliente.ts            ← instância única do DatabaseSync
      UsuarioRepositorio.ts
      TarefaRepositorio.ts
```

## Consequências

- Zero dependências externas para acesso ao banco de dados.
- SQL explícito e legível dentro dos repositórios — sem camada de tradução ou magia de ORM.
- Tipagem garantida por interfaces TypeScript nas assinaturas dos repositórios.
- Trocar de banco de dados implica reescrever apenas os repositórios; o restante do código não muda.
- Alinhado com Node.js 24.16.0 LTS, versão adotada no projeto.
