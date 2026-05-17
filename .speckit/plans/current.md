---
dono: Cesar
atualizado: 2026-05-17
status: ativo
---

# Plano Atual — Atelier

## Iteração: M0 — Foundation Mora over open-design fork
**Início:** 2026-05-17
**Critério de fechamento:** Foundation Mora commitada e pushada pra `Czar210/Atelier`; manifesto + Director + speckit + 4 ADRs aprovados; description do repo atualizada.

### Entregas
- [x] Fork criado em GitHub: `Czar210/Atelier` (a partir de `nexu-io/open-design`)
- [x] Clone local em `c:/Users/cesar/Documents/GitHub/Atelier/`
- [x] `README.upstream.md` preservado (README original open-design)
- [x] `.gitignore` estendido com Mora patterns (*.zip, Atelier-handoff*)
- [x] `manifesto.md` criado (Mora-flavored, 8 seções)
- [x] `README.md` reescrito (Atelier Mora-flavored com attribution upstream)
- [x] `UPSTREAM.md` criado (sync tracking)
- [x] `CLAUDE.md` criado (operacional, padrão Mora)
- [x] `CONTEXT_DIRECTOR.md` criado (§1-6, stack travada, regras duras §4)
- [x] `.speckit/` estrutura completa criada
- [x] `.speckit/product/vision.md` + `modes-spec.md`
- [x] 4 ADRs ativos:
  - ADR-0001 fork open-design vs from-scratch
  - ADR-0002 local-first Tauri stack
  - ADR-0003 bundle impeccable + taste-skill como core
  - ADR-0004 modes Studio default + Vereda opt-in (invertido vs Strata)
- [ ] Cesar revisa rascunhos (manifesto, vision, Director) e edita
- [ ] `gh repo edit Czar210/Atelier --description="..."`
- [ ] Commit `M0: Mora foundation on open-design fork` + push

## Próxima iteração (preview)

**M0.5 — DS interno do Atelier (a decidir)**

Pergunta aberta: Atelier UI usa DS herdado do upstream open-design (rebrandeado) OU cria DS próprio via Claude Design (igual Strata)?

Argumentos pro DS próprio:
- Identidade Atelier Mora-flavored única
- Coerência com Strata (cada produto Mora tem manifesto + DS próprio)
- Pode reusar conceitos editoriais do Strata DS v2 (Fraunces, warm grounds) com adaptações pra Atelier

Argumentos pra herdar:
- Velocidade — open-design já tem DS funcional
- Menos manutenção
- Atelier pode "rebrandear" via overrides CSS

Decisão pendente. Discussão em ADR-0005 quando entrar M0.5.

**M1 dev — primeiro código Mora-flavored sobre o fork**
- Criar `apps/desktop-tauri/` (Tauri wrapper sobre o web app upstream)
- Wire Studio/Vereda toggle (UI mínima)
- Wire impeccable + taste-skill loader obrigatório
- Smoke test: gerar prototype simples em Studio, ativar Vereda, gerar nota mock
- Sem DS próprio ainda (vem em M0.5+M2)
