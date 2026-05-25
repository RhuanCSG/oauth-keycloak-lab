# Domínio: Task Manager

## Visão Geral

O domínio do projeto é um **Task Manager** — sistema de gerenciamento de tarefas. Foi escolhido por ser simples o suficiente para não desviar o foco dos fluxos OAuth, mas realista o suficiente para justificar papéis, escopos e múltiplos tipos de client.

## Atores

| Ator | Tipo | Descrição |
|---|---|---|
| **Usuário** | Humano autenticado | Cria e gerencia suas próprias tarefas via SPA |
| **Admin** | Humano autenticado com papel elevado | Visualiza e gerencia tarefas de todos os usuários |
| **Worker** | Serviço (sem usuário) | Acessa todas as tarefas e gera relatório agregado via Client Credentials |

## Recurso Principal: Tarefa

| Campo | Tipo | Obrigatoriedade | Descrição |
|---|---|---|---|
| `id` | UUID | Gerado pelo sistema | Identificador único |
| `titulo` | texto | Obrigatório | Título da tarefa |
| `descricao` | texto | Opcional | Detalhamento da tarefa |
| `status` | enum | Obrigatório | `a_fazer` · `fazendo` · `feito` |
| `usuario_id` | UUID | Gerado pelo sistema | Referência ao dono da tarefa |

## Escopos OAuth

Os escopos emergem diretamente das operações do domínio:

| Escopo | Quem usa | O que autoriza |
|---|---|---|
| `tarefas:leitura` | Usuário, Admin | Leitura de tarefas |
| `tarefas:escrita` | Usuário, Admin | Criação, atualização e exclusão de tarefas |
| `relatorios:leitura` | Worker | Leitura de todas as tarefas para geração de relatório |
