# ADR-000 — Formato e localização dos registros de decisão

- **Status:** Aceito
- **Data:** 2026-05-25

## Contexto

O projeto envolve múltiplas decisões técnicas e arquiteturais ao longo de 6 fases. Sem um registro estruturado, o raciocínio por trás de cada escolha se perde, dificultando a manutenção e o entendimento por outros desenvolvedores.

## Decisão

Adotar o formato MADR (Markdown Architectural Decision Records) para registrar decisões. Cada ADR é um arquivo Markdown com: contexto, decisão, consequências e status.

**Localização:** `docs/decisions/` na branch `main`.  
**Nomenclatura:** `ADR-NNN-titulo-curto.md`, com numeração sequencial a partir de 000.

## Consequências

- Toda decisão relevante de escopo, arquitetura, tecnologia ou processo gera um ADR antes de ser implementada.
- ADRs ficam centralizados na `main` e são independentes das branches de fase.
- Status possíveis: `Proposto`, `Aceito`, `Substituído`, `Depreciado`.
