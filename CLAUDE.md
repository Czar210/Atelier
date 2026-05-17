# CLAUDE.md — Atelier

## Meu Papel
Sou o **Programador** do Atelier. Cesar é o Diretor — ele decide o que construir e aprova cada entrega. Não existe planejador externo neste projeto: Cesar planeja, eu executo, TestSprite valida.

**Fluxo padrão:** Cesar descreve o que quer → eu proponho a abordagem → Cesar aprova → eu implemento → TestSprite testa → Cesar aprova.

**Antes de propor abordagem:** consulto [`CONTEXT_DIRECTOR.md`](CONTEXT_DIRECTOR.md) (regras, stack, regras duras §4), [`UPSTREAM.md`](UPSTREAM.md) (estado do fork vs open-design) e [`.speckit/plans/current.md`](.speckit/plans/current.md) (iteração atual). Desvio das §3/§4 do Director = bloqueador, não execução silenciosa.

**Upstream guidance:** o open-design tem seu próprio [`AGENTS.md`](AGENTS.md) com instruções pra agents. Quando eu mexer em código upstream (`apps/`, `packages/`), consulto AGENTS.md também — ele tem convenções específicas do open-design que precisam ser respeitadas pra sync futuro.

**Antes de escrever UI/CSS:** Atelier ainda não tem DS próprio (M0.5 futuro). Por enquanto, herda o que open-design upstream traz. Quando criarmos DS próprio (potencialmente via Claude Design igual Strata), documento aqui.

---

## A Filosofia do Produto (leia antes de qualquer código)

Atelier tem dois modos. **Invertido do Strata.**

```
MODO STUDIO (padrão)
  - Cria. Gera. Audita. Polia.
  - Design systems novos + prototypes + slides + image + video
  - impeccable + taste-skill bundled e sempre carregados
  - Output: artefatos exportáveis (HTML/PDF/PPTX/MP4)

MODO VEREDA (opt-in com fricção)
  - Explica princípios de design, aponta referências primárias
  - Gera nota Obsidian se user tem vault (opcional, não vital)
  - Não produz artefatos — produz entendimento
```

**Por que invertido vs Strata:** design começa fazendo (iteração rápida é a prática), código começa entendendo (refletir antes de mudar é a prática). Cada produto Mora respeita o ritmo do seu domínio.

Toda feature nova respeita essa distinção. Se uma feature colapsa os dois modos, ela quebra o produto.

---

## Stack

- **Core:** Fork de [open-design](https://github.com/nexu-io/open-design) (TypeScript + pnpm)
- **Agent delegation:** Pi-style PATH scan (herdado do upstream)
- **GUI:** Tauri 2 + React + TypeScript (wrapper Mora-only — web app upstream preservado)
- **Skills bundled e obrigatórias:** impeccable (audit) + taste-skill (consistency)
- **Modelos locais:** Ollama
- **Modelos cloud:** opt-in (Anthropic, OpenAI, Gemini, etc. herdado do upstream)
- **Vault (opcional):** Obsidian — markdown com frontmatter YAML
- **Testes:** Vitest (unit) + Playwright (e2e) + TestSprite (QA)

---

## Como Rodar Localmente

```bash
# Instalar deps (open-design usa pnpm)
pnpm install

# Dev
pnpm dev

# Build
pnpm build
```

Ver [`QUICKSTART.pt-BR.md`](QUICKSTART.pt-BR.md) (do upstream) pra mais detalhes operacionais.

Ollama deve estar rodando em `http://localhost:11434` para modelos locais.

---

## Arquitetura

```
Atelier/
├── apps/                   # do upstream open-design — daemon, web app
├── packages/               # do upstream — skills, design systems, adapters
├── manifesto.md            # filosofia Mora
├── README.md               # overview Atelier Mora-flavored
├── README.upstream.md      # README original open-design (preservado)
├── UPSTREAM.md             # tracking do sync com open-design
├── CLAUDE.md               # este arquivo
├── CONTEXT_DIRECTOR.md     # regras, stack, regras duras §4
├── AGENTS.md               # guidance do upstream pra agents (preservado)
├── .speckit/               # specs vivas, planos, tracking, ADRs Mora
├── .gitignore              # upstream + Mora patterns
└── LICENSE                 # Apache-2.0 (preservada)
```

---

## Convenções de Código

Atelier herda convenções do open-design upstream (TypeScript, pnpm workspaces). Convenções Mora-específicas:

### Regra fundamental
**Tipos e contratos antes de qualquer implementação.** Nenhuma função sem sua interface TypeScript definida primeiro.

### Nomenclatura (Mora)
| Padrão | Uso | Exemplo |
|--------|-----|---------|
| `is*` / predicado bare | Booleanos | `isStudioMode`, `vaultLoaded` |
| `_método` | Privado | `_renderPrototype`, `_auditDesign` |
| `SCREAMING_SNAKE` | Constantes | `DEFAULT_TASTE`, `IMPECCABLE_DETECT_PATH` |
| `use*` + substantivo | Hooks React | `useAtelierStore`, `useImpeccable` |
| sem sufixo Async | Funções async | `generate()`, `audit()` |

### Comentários
- Esparsos e estratégicos: explica o **por quê**, não o **o quê**
- Sem docstrings multi-linha
- Português para comentários Mora, inglês para código (consistência com upstream)

### Erros
- Falhas silenciosas em I/O secundário (vault opcional, telemetria) — não travar fluxo principal
- Falhas explícitas em contratos quebrados (tipos inválidos, skill faltando) — errar rápido
- Sem try/catch genérico que engole erros

---

## Integração com o Vault (opcional)

Atelier **não exige** vault Obsidian (diferente de Strata onde é central). Se configurado, notas Vereda vão pra `inbox/` mesmo formato Strata:

```markdown
---
tags: [atelier, design, {area}]
bloom: {1-6}
data: {YYYY-MM-DD}
refs: [{referência primária de design}]
relacionados: [[{nota anterior}]]
---

## {Princípio}

{Explicação em prosa}
```

Vault path configurado em `~/.atelier/settings.json`.

---

## Bundled Skills (obrigatórias)

| Skill | Função | Como Atelier usa |
|---|---|---|
| **impeccable** | Audit/critique/polish (23 commands + 27 anti-patterns deterministicos) | Roda em todo output do Modo Studio antes de mostrar ao user |
| **taste-skill** | Taste consistency (11 variants + dials) | Configura dial inicial baseado em DS escolhido; mantém coerência inter-artefatos |

**Não são opcionais.** Atelier sem elas quebra a proposta "discipline embarcada". Ver [ADR-0003](.speckit/architecture/adr/ADR-0003-bundle-impeccable-and-taste-skill.md).

---

## O que EU não faço neste projeto

- Não decido divergir do upstream open-design sem ADR
- Não mudo nome do package (atelier vs open-design) sem aprovação — afeta CI, releases
- Não removo skills bundled (impeccable, taste-skill)
- Não removo Modo Vereda em favor de Studio puro
- Não removo attribution upstream
- Não tomo decisões de produto sem aprovação do Cesar
- Não adiciono dependências sem justificativa explícita
