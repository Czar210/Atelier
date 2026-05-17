# Patch Notes — Atelier

Mudanças com impacto observável. Granularidade: alguém externo deveria conseguir entender o que mudou só lendo aqui.

## 2026-05-17 — M0 fundação documental Mora over open-design fork
- Fork criado em `Czar210/Atelier` a partir de `nexu-io/open-design` (Apache-2.0)
- Clone local em `c:/Users/cesar/Documents/GitHub/Atelier/`
- Upstream estado preservado: SHA base `6bf865a4`, branch `main`, latest tag `open-design-v0.7.0`
- README upstream movido pra `README.upstream.md`; novo `README.md` Mora-flavored com attribution
- `.gitignore` estendido com Mora patterns: `*.zip`, `Strata Design System*`, `Atelier-handoff*`
- Documentos Mora criados:
  - `manifesto.md` — 8 seções, posicionamento "designs que você decide"
  - `CLAUDE.md` — operacional pra Claude Code (sobrescreveu placeholder 12-byte do upstream)
  - `CONTEXT_DIRECTOR.md` — §1-6 com stack travada e regras duras §4
  - `UPSTREAM.md` — sync tracking com política cherry-pick
- `.speckit/` estrutura completa:
  - `product/vision.md` + `modes-spec.md`
  - 4 ADRs ativos (fork, Tauri stack, bundle skills, modes invertidos)
  - `plans/current.md` + `backlog.md` + `done.md`
  - `tracking/decisions.md` + `patch-notes.md` + `bugfixes.md`
- Sem código Mora-flavored ainda — só base documental. M1 dev começa quando Cesar aprovar foundation.
- M0 fecha quando: Cesar revisa + edita rascunhos + aprova + primeiro commit/push pushed.
