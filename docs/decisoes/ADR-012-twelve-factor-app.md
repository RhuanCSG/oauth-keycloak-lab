# ADR-012 — Aderência ao Twelve-Factor App

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O Twelve-Factor App é uma metodologia para construir aplicações modernas e escaláveis. Nem todos os fatores são relevantes para um projeto de portfólio e estudo focado em fluxos OAuth.

## Decisão

Aplicar os **fatores de alta relevância como princípio de design**, sem tratar a metodologia como objetivo explícito.

### Fatores aplicados

| Fator | Aplicação |
|---|---|
| **II. Dependências** | Todas declaradas em `package.json`; nenhuma dependência implícita do ambiente |
| **III. Configuração** | Toda configuração via variáveis de ambiente (`.env`); zero valores hardcoded |
| **IV. Backing services** | Keycloak e SQLite referenciados via env vars — substituíveis sem mudança de código |
| **VI. Processos** | Serviços stateless — nenhuma sessão ou estado armazenado em memória de processo |
| **VII. Port binding** | Porta de cada serviço configurável via variável de ambiente |
| **X. Paridade dev/prod** | Docker garante consistência entre ambientes |
| **XI. Logs** | Saída para stdout — sem gerenciamento de arquivo de log |

### Fatores ignorados
VIII (Concorrência) e XII (Processos administrativos) estão fora do escopo do projeto.

## Consequências

- Nenhum valor de configuração aparece hardcoded no código — tudo vem de `process.env`.
- Os serviços podem ser parados, iniciados e substituídos sem efeitos colaterais de estado.
- Os fatores aplicados são mencionados no README geral como decisão de arquitetura, sem criar cerimônia adicional.
