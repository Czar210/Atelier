---
dono: Cesar
atualizado: 2026-05-17
status: ativo
supersedes: nenhum
superseded_by: nenhum
---

# ADR-0002 — Local-first com Tauri (mesma stack Strata)

## Contexto

Open-design upstream é primariamente uma web app + daemon (Node.js). Roda local mas via browser. Apresenta artefatos em sandboxed iframe.

Atelier herda esse paradigma mas Mora prefere binário desktop nativo (postura local-first explícita, não "rodando no browser que precisa estar aberto"):

- Strata já decidiu Tauri 2 (ADR-0002 do Strata)
- Cesar conhece a stack
- Tauri ~10MB binary vs Electron ~150MB — alinha com manifesto §VI

Pergunta: Atelier preserva web app do upstream OU adiciona/substitui com Tauri 2 desktop wrapper?

## Decisão

**Adicionar Tauri 2 desktop wrapper** sobre o core do open-design. Web app do upstream permanece funcional (alguém pode rodar Atelier no browser se preferir), mas o caminho canônico Mora é binário Tauri.

Stack Atelier:
- **Tauri 2** (Rust + WebView) — desktop wrapper canonical Mora
- **Vite + React 19 + TypeScript** — frontend (mesma stack Strata)
- **TailwindCSS v3** — styling
- **Daemon Node.js** — herdado do upstream open-design (Atelier consome a API local do daemon)
- **pnpm workspaces** — herdado do upstream (não tentar mudar pra npm)

## Consequências

### Positivas
- Binário enxuto (~10-15MB esperado) — coerente com Strata
- Postura local-first explícita (não "abra Chrome em localhost:xxxx")
- Stack idêntica ao Strata = expertise reutilizável + componentes compartilháveis futuros
- Web app upstream preservado = quem quer pode rodar sem Tauri
- Tauri pode bridge pro daemon Node.js via tauri command (`invoke()` chamando API local)

### Negativas
- **Dupla manutenção:** web app upstream + Tauri wrapper Mora-flavored
- **Refactor do upstream UI** pra rodar em WebView restrita do Tauri (algumas APIs browser-only podem não funcionar)
- **MSVC no Windows** (mesma dor que Strata terá) — Cesar precisa Visual Studio Build Tools instalado pra `tauri build`

## Mitigações

- **Não tocar no web app upstream** — vive em `apps/web/` (ou onde o upstream colocou), preservado pra sync
- **Tauri wrapper vive em `apps/desktop-tauri/`** (nova pasta Mora) — espelha lógica do web app via Vite + React + TS
- **MSVC no Windows:** quando começar M1 dev, validar setup; documentar requisitos em README
- **Reavaliar em 6 meses:** vale continuar dupla UI ou congelar web app upstream?

## Stack table (resumo)

| Camada | Tecnologia | Origem |
|---|---|---|
| Wrapper desktop | Tauri 2 (Rust + WebView) | Mora-only |
| Frontend | Vite + React 19 + TS + Tailwind v3 | Mora-only (mesmo Strata) |
| Daemon (backend local) | Node.js (TypeScript) | herdado do upstream |
| Estado client | Zustand (a confirmar) | Mora-only quando integrarmos |
| Skills | impeccable + taste-skill (sempre) + outras opcionais herdadas | bundled obrigatório (Mora) + opcional (upstream) |
| Modelos | Ollama (padrão) + cloud opt-in | herdado do upstream |
| Sandboxed preview | iframe (herdado do upstream) | upstream |
| Export | HTML/PDF/PPTX/MP4/ZIP (herdado) | upstream |

## Relação com outros ADRs
- [ADR-0001](ADR-0001-fork-open-design-vs-from-scratch.md) decidiu fork — habilitou ter Tauri wrapper sobre infraestrutura existente
- ADR-0003 (bundle skills) é independente desta decisão
- ADR-0004 (modes) usa essa stack como base
