# Context Director — Atelier

> **Para as IAs (sistema):** leia este documento antes de propor mudanças estruturais. Confirme contra o disco — não invente. Este doc descreve **como trabalhamos**, não a filosofia (essa está em [`manifesto.md`](manifesto.md)) nem o backlog atual (esse está em [`.speckit/`](.speckit/)).

---

## §1. Identidade e Time

Definição operacional vive em [`CLAUDE.md`](CLAUDE.md). Em resumo:

| Papel | Quem | Escopo |
|---|---|---|
| Diretor | Cesar | Visão, prioridade, aprovação |
| Programador | Claude Code | Executa o aprovado, lê disco antes de propor |
| QA | TestSprite | Testes e2e, integração, cenários de borda |
| Designer (DS interno do Atelier, quando vier) | Claude Design | Sistema visual da UI da própria app |

IAs **não decidem mudança de stack, arquitetura ou divergência de upstream sozinhas**. Levantam como bloqueador e esperam aval.

---

## §2. Mentalidade Arquitetural

Atelier é **local-first**, **creation-first**, **discipline-embarcada**. Toda decisão técnica passa por essas três lentes.

- **Tipos antes de código.** Convenção herdada do upstream + reforçada por Mora.
- **Studio como padrão é inquebrável.** Nenhuma feature pode tornar Modo Vereda o caminho natural. Toggle ambíguo que confunde os dois modos = bug arquitetural.
- **Offline é postura, não fallback.** Ollama local é o caminho default. Cloud é opt-in com configuração explícita (herdado do upstream).
- **Vault é opcional, nunca obrigatório.** Usuário pode usar Atelier sem nenhum vault. Quando configurado, comportamento idêntico ao Strata (`inbox/` é território do Atelier por contrato).
- **Sem fallback silencioso entre providers.** Se Ollama cai, falha clara. Cloud não é tentada por "conveniência" sem opt-in.
- **Skills bundled são contrato.** impeccable + taste-skill carregam sempre em Studio. Disable = produto diferente. Mudança = ADR.
- **Atomicidade na geração.** Se gera prototype mas falha ao exportar, usuário vê o erro — não fica meio-arquivo órfão.
- **Observabilidade.** Log estruturado JSON, mascarar credenciais cloud, nunca logar conteúdo gerado (privacidade do brand do user).

---

## §3. Stack Travada (mudar só com aval explícito)

| Camada | Escolha | Por quê |
|---|---|---|
| Core | Fork do [open-design](https://github.com/nexu-io/open-design) (Apache-2.0) | Já tem infraestrutura madura (agent delegation, sandboxed preview, 19 skills + 71 DS, export multi-formato). Reimplementar = meses de trabalho duplicado. Forkar mantém upstream sync possível. ADR-0001. |
| Package manager | pnpm | Herdado do upstream — open-design é pnpm workspace. Mudar = refactor massivo sem ganho. |
| Agent delegation | Pi-style (PATH scan por agents) | Herdado do upstream — pattern já maduro, suporta multi-agent (claude, codex, gemini, cursor-agent, etc.). |
| GUI | Tauri 2 + React + TS | Coerência com Strata (mesma stack, mesma postura local-first). Open-design upstream usa web app — Atelier substitui por Tauri pra binário local + offline-first. Decisão em ADR-0002. |
| Modelos locais | Ollama (`localhost:11434`) | Padrão Mora. Mesma escolha que Strata. |
| Modelos cloud | Anthropic, OpenAI, Gemini, Azure (via upstream) | Opt-in. Herdado do upstream. |
| Vault (opcional) | Obsidian (markdown + YAML) | Quando configurado, formato canônico igual Strata. **Opcional**, nunca obrigatório (diferente de Strata onde é central). |
| Skills bundled obrigatórias | impeccable + taste-skill | Discipline embarcada — sem elas, Atelier vira slop generator. ADR-0003. |
| Testes | Vitest (unit) + Playwright (e2e) + TestSprite (QA) | Mesma stack Strata. Cesar familiar. |
| Modos | Studio (default) + Vereda (opt-in) | Invertido de Strata. Design começa fazendo. ADR-0004. |

---

## §4. Regras Duras de Produto (não-negociáveis)

1. **Studio é o padrão. Sempre.** Modo Vereda é opt-in com fricção deliberada. Toggle persistente entre sessões = quebra a filosofia.
2. **impeccable e taste-skill sempre carregam em Studio.** User não pode desligar via config. Mudança = ADR.
3. **Atelier nunca edita vault existente sem permissão explícita.** Quando vault configurado, cria em `inbox/` apenas (mesmo contrato Strata).
4. **Sem fallback silencioso de provider.** Cadeia pública (Ollama → provider configurado → erro). Ordem nunca pulada sem o usuário saber.
5. **Frontmatter da nota Vereda segue schema único** (mesmo schema Strata: `tags`, `bloom`, `data`, `refs`, `relacionados`). Mudança = ADR Mora-wide (afeta ambos produtos).
6. **Telemetria é opt-in.** Nada sai da máquina sem o usuário ligar.
7. **Attribution upstream preservada.** README, LICENSE, copyright headers do open-design intocados (excepto README que foi reescrito mantendo attribution clara).
8. **Exports sempre vão pro disco do user**, nunca pra cloud sem opt-in. Padrão `.atelier/projects/<id>/` herdado do upstream.

---

## §5. Mapa do Ecossistema

Estado em **2026-05-17**:

```
Atelier/
├── (estrutura upstream open-design preservada — apps/, packages/, etc.)
├── manifesto.md            [novo] filosofia Mora
├── README.md               [novo] overview Atelier Mora-flavored
├── README.upstream.md      [preservado] README original open-design
├── UPSTREAM.md             [novo] sync tracking
├── CLAUDE.md               [novo, sobrescreveu placeholder] instruções operacionais
├── CONTEXT_DIRECTOR.md     [novo] este documento
├── .speckit/               [novo] specs vivas Mora
├── .gitignore              [estendido] upstream + Mora patterns
├── AGENTS.md               [preservado] agent guidance upstream
├── LICENSE                 [preservado] Apache-2.0 upstream
└── CONTRIBUTING/MAINTAINERS/QUICKSTART (6 línguas)  [preservado]
```

Próximo marco: M0.5 — definir se Atelier UI vai ter DS próprio (Claude Design igual Strata) ou herdar visual do upstream open-design adaptado. Decisão em backlog.

---

## §6. Como cada IA consulta este doc

- **Claude Code (eu):** antes de propor qualquer feature, confirmo alinhamento com §2 (mentalidade), §3 (stack), §4 (regras duras). Desvio = bloqueador, não execução silenciosa. **Antes de tocar em código upstream:** verifico `UPSTREAM.md` pra entender se a mudança vai conflitar com sync futuro.
- **TestSprite:** cenários obrigatórios derivados de §4 — testar que Studio é default em toda nova sessão; que impeccable roda antes de exibir output; que vault não-configurado não bloqueia uso; que Ollama down sem cloud configurado = erro claro.
- **Claude Design (DS interno futuro):** segue padrão Mora estabelecido em Strata. DS do Atelier vive em `design/` (paralelo ao código), com `colors_and_type.css` canônico + screens em `ui_kits/`.

---

*Atualize este documento quando uma decisão macro mudar. Decisões pontuais vão pra [`.speckit/architecture/adr/`](.speckit/architecture/adr/).*
