# Regras de Negócio

## Operações por Papel

### Usuário comum
- Criar tarefas próprias
- Listar suas próprias tarefas
- Atualizar suas próprias tarefas (título, descrição, status)
- Excluir suas próprias tarefas

### Admin
- Todas as operações do usuário comum
- Listar tarefas de todos os usuários
- Excluir tarefas de qualquer usuário

### Worker (Client Credentials)
- Listar todas as tarefas do sistema (somente leitura)
- Gerar relatório de status agregado: total por status (`a_fazer`, `fazendo`, `feito`)

## Regras de Isolamento

- Um usuário nunca acessa tarefas de outro usuário, mesmo que conheça o ID da tarefa.
- O papel `admin` é identificado por claim no token JWT — nunca por lógica de aplicação ou consulta ao banco.
- O worker é identificado pela ausência de `sub` de usuário no token (token de serviço, não de usuário).

## Transições de Status

O status de uma tarefa segue um ciclo linear sem restrições de transição:

```
a_fazer → fazendo → feito
```

Qualquer transição é permitida — o usuário pode mover uma tarefa de `feito` de volta para `a_fazer` se necessário.

## Proteção de Rotas

Toda rota protegida exige a combinação:
1. **Autenticação:** token JWT válido (não expirado, assinatura verificada)
2. **Autorização:** papel ou escopo adequado para a operação solicitada

Uma falha de autenticação retorna `401 Unauthorized`.
Uma falha de autorização retorna `403 Forbidden`.
