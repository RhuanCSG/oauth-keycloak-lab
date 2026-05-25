# ADR-009 — Cliente HTTP para testes manuais

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

As fases sem frontend (1, 2 e 3) exigem um cliente HTTP para testar as APIs durante o desenvolvimento e para documentar os fluxos de forma reproduzível.

## Decisão

Usar **duas abordagens complementares**:

**Arquivos `.http`**
- Versionados no repositório de cada fase, dentro de `docs/http/`
- Cobrem todos os fluxos da fase: autenticação, rotas protegidas, casos de erro
- Executáveis via REST Client (VSCode) sem dependência externa
- Fonte de verdade para reproduzir qualquer fluxo a partir do repositório

**Postman**
- Collection exportada (`postman_collection.json`) versionada em `docs/http/`
- Usado para exploração interativa, demonstrações e testes manuais mais ricos
- Variáveis de ambiente configuradas para apontar para o ambiente local

## Consequências

- Qualquer desenvolvedor consegue reproduzir todos os fluxos sem instalar ferramentas além do VSCode ou do Postman.
- Os arquivos `.http` servem como documentação viva das APIs de cada fase.
- A collection Postman complementa para cenários que exigem interface mais rica (ex: fluxos OAuth com redirect).
