# Upstream Sync — Atelier ← open-design

Atelier é fork/upgrade Mora-flavored do [open-design](https://github.com/nexu-io/open-design) (Apache-2.0). Este arquivo rastreia commits do upstream já incorporados, decisões de divergência, e plano de sync futuro.

---

## Estado inicial

| Item | Valor |
|---|---|
| Fork criado | 2026-05-17 |
| Repo Atelier | https://github.com/Czar210/Atelier |
| Upstream | https://github.com/nexu-io/open-design |
| Branch base | `main` |
| Commit base do fork | `6bf865a4` — "fix(ci): avoid duplicate nix-check runs on PR branches (#1917)" |
| Versão open-design no fork | 0.7.0 (latest tag: `open-design-v0.7.0`) |
| Licença | Apache-2.0 (preservada) |

## Política de sync

- **Sync periódico:** revisar upstream a cada release major do open-design (não a cada commit)
- **Cherry-pick > merge full:** quando upstream evoluir, escolher mudanças relevantes em vez de mergear tudo (Atelier diverge intencionalmente)
- **Conflitos esperados em:** README.md (Atelier reescreveu), package.json (quando Atelier renomear), arquivos Mora-only (manifesto.md, CONTEXT_DIRECTOR.md, .speckit/)
- **Conflitos zero esperados em:** código de `apps/`, `packages/`, skills bundled

## Divergências Mora-flavored aplicadas

| Mudança | Arquivo | Justificativa |
|---|---|---|
| Reescrita do README | `README.md` (upstream → `README.upstream.md`) | Identidade Atelier Mora-flavored, attribution preservada |
| Foundation Mora adicionada | `manifesto.md`, `CONTEXT_DIRECTOR.md`, `CLAUDE.md`, `.speckit/` | Padrão de governance Mora (Director + speckit + manifesto) |
| `.gitignore` estendido | append Mora patterns (*.zip, design backups) | Mora artifacts não vão pro repo |
| Modos Studio + Vereda | (implementação pendente — ver `.speckit/product/modes-spec.md`) | Inversão vs Strata (criação default, ensino opt-in) |
| impeccable + taste-skill bundled obrigatórios | (implementação pendente — ver ADR-0003) | Discipline embarcada — não opcionais |

## Histórico de sync

| Data | Commits incorporados | Notas |
|---|---|---|
| 2026-05-17 | Fork inicial até `6bf865a4` | Estado base — open-design v0.7.0 + 4 commits subsequentes |

(append entries futuras aqui)

---

## Como sincar (cookbook)

```bash
# Add upstream remote (uma vez)
git remote add upstream https://github.com/nexu-io/open-design.git

# Fetch upstream
git fetch upstream

# Ver o que mudou
git log HEAD..upstream/main --oneline

# Cherry-pick commits desejados
git cherry-pick <sha>

# OU merge controlado (raro)
git merge upstream/main --no-commit
# resolver conflitos esperados em README, package.json, etc.
git commit -m "sync: upstream open-design @ <sha>"
```

Depois de sync, atualizar este arquivo com:
- Range de commits incorporados (de SHA → para SHA)
- Conflitos resolvidos
- Mudanças Mora que precisaram ser re-aplicadas
