---
dono: Cesar
atualizado: 2026-05-17
status: ativo
supersedes: nenhum
superseded_by: nenhum
---

# ADR-0001 — Forkar open-design vs reimplementar do zero

## Contexto

Atelier precisa de uma plataforma de geração de design local-first com agent delegation (chamar Ollama/Claude/Codex/etc. via PATH scan), sandboxed preview, export multi-formato (HTML/PDF/PPTX/MP4) e sistema de skills/design-systems plugáveis.

Três opções avaliadas:

1. **Reimplementar do zero** em TypeScript+Tauri
2. **Forkar [open-design](https://github.com/nexu-io/open-design)** (Apache-2.0, já tem 19 skills + 71 DS bundled)
3. **Usar open-design como dependência** sem fork

## Decisão

**Forkar.**

Mesma lógica do ADR-0001 do Strata (forkar Pi): plataforma upstream tem infraestrutura madura, custo de reimplementação é prazo de meses, fork permite Mora-flavored sem perder upstream evolution.

## Consequências

### Positivas
- Herdamos imediatamente:
  - Agent delegation pattern (scaneia PATH por claude/codex/gemini/cursor-agent/qwen)
  - Sandboxed iframe preview
  - Export multi-formato (HTML/PDF/PPTX/MP4/ZIP)
  - Sistema de skills plugáveis (19 skills já existem)
  - 71 design systems bundled como starting points
  - MCP server pra que outros agents leiam projetos Atelier
  - Daemon + CLI (`od`) base
- Apache-2.0 permite fork sem restrição
- Mora-flavored vira "Atelier — fork/upgrade Mora de open-design", identidade clara
- Sync upstream possível (cherry-pick) quando open-design evoluir

### Negativas
- **Identidade de fork:** Atelier carrega "fork of open-design" — sempre tem upstream pra atribuir
- **Maintenance burden:** trackar upstream commits, decidir o que mergear, resolver conflitos
- **Decisões upstream viram nossas por inércia** se não revisitarmos (ex: pnpm, web app vs Tauri, etc.)
- **Versão pode divergir** — upstream em v0.7.0, Atelier vai começar a sua própria versão Mora-flavored em iteração futura

## Mitigações

- Manter [`UPSTREAM.md`](../../../UPSTREAM.md) listando commits do open-design já mergeados + estado base
- Política: cherry-pick > merge full (Atelier diverge intencionalmente em UX/branding)
- Reservar `apps/` e `packages/` do upstream como zona "upstream code"; código Mora-only vive em `.speckit/`, `manifesto.md`, `CONTEXT_DIRECTOR.md`, e arquivos Mora-specific futuros (ex: `apps/desktop-tauri/` quando criarmos)
- Reavaliar em 6 meses (2026-11): vale continuar sync ou congelar upstream e divergir totalmente?

## Relação com upstream

Atelier mantém atribuição clara:
- LICENSE Apache-2.0 preservada
- README original em `README.upstream.md`
- `UPSTREAM.md` rastreia sync
- Headers de copyright em arquivos upstream intocados
