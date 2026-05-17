# Decisões Macro — Atelier

Log cronológico. Decisões com impacto arquitetural geram ADR (`.speckit/architecture/adr/`); este arquivo aponta pra elas.

## 2026-05-17 — fundação Mora over open-design fork
- **Atelier nasce como fork/upgrade Mora-flavored de [open-design](https://github.com/nexu-io/open-design)** (Apache-2.0). Fork em `Czar210/Atelier`. Mesma lógica do ADR-0001 do Strata (fork upstream maduro vs reimplementar do zero).
- **Adotada estrutura Director + speckit + CLAUDE.md em 3 camadas.** Padrão Mora estabelecido em Strata aplicado aqui. Manifesto = por quê, Director = como, speckit = o quê.
- **ADR-0001 (ativo):** forkar open-design (vs reimplementar do zero ou usar como dep).
- **ADR-0002 (ativo):** local-first com Tauri 2 — adicionamos wrapper desktop ao web app upstream. Mesma stack Strata.
- **ADR-0003 (ativo):** impeccable + taste-skill bundled como core (não opcionais). Discipline embarcada — manifesto §IV materializado.
- **ADR-0004 (ativo):** modos Studio (default) + Vereda (opt-in). **Invertido vs Strata.** Razão: design começa fazendo, código começa entendendo. Cada produto Mora respeita o ritmo do seu domínio.
- **Vault Obsidian é opcional** (diferente de Strata onde é central). Atelier funciona sem vault. Quando configurado, formato canônico igual Strata (`inbox/`, frontmatter idêntico).
- **README upstream preservado** como `README.upstream.md`. Attribution mantida em LICENSE + README Atelier.
- **`UPSTREAM.md`** rastreia sync. Política: cherry-pick > merge full. Atelier diverge intencionalmente em UX/branding.
- **Skills bundled obrigatórias:** impeccable + taste-skill carregam sempre em Studio. User não pode desligar via config. Mudança = ADR.
- **Strata ↔ Atelier integration:** decisão de **deixar pra futuro** (M3+). Os dois podem conversar via vault compartilhado, mas não da arquitetura inicial.
