---
dono: Cesar
atualizado: 2026-05-17
status: ativo
supersedes: nenhum
superseded_by: nenhum
---

# ADR-0003 — Bundle impeccable + taste-skill como dependências core (não opcionais)

## Contexto

Manifesto Atelier (§IV) afirma: *"discipline embarcada — você não consegue gerar design genérico mesmo se quiser"*. Pra concretizar isso, Atelier precisa de:

- **Audit/critique** automático em todo output do Modo Studio
- **Taste consistency** garantida entre artefatos de um projeto

Duas skills externas (Apache-2.0 / MIT) preenchem esses dois papéis:

- **[impeccable](https://github.com/pbakaus/impeccable)** — 23 commands + 27 anti-patterns deterministicos. Modo CLI: `npx impeccable detect src/` sem necessidade de API call.
- **[taste-skill](https://github.com/leonxlnx/taste-skill)** — 11 design tastes (brutalist/minimalist/soft/etc.) + dials (variance/motion/density 1-10).

Pergunta: essas skills entram como (a) opt-in que user instala depois, (b) recommended in docs mas não obrigatório, (c) bundled e obrigatório (carregam sempre, user não pode desligar)?

## Decisão

**Opção (c): bundled e obrigatório.** impeccable e taste-skill carregam SEMPRE em Modo Studio. User não pode desligar via config. Mudança = ADR.

## Consequências

### Positivas
- **Manifesto materializado.** Discipline não é "opção pra quem se importa" — é o produto.
- **Output Atelier é distinguível.** Quem usa, internaliza padrões. Quem migra pra outra ferramenta sente falta.
- **Onboarding sem fricção.** Skills já estão lá, user não precisa configurar.
- **impeccable em mode CLI determinístico** (sem API call) = funciona offline, alinha com postura local-first.

### Negativas
- **Maintenance burden:** sync com upstream das 2 skills (versionar, testar compat).
- **Risco de conflito com DS interno do Atelier futuro:** algumas regras impeccable (ex: "use system fonts") podem conflitar com decisões DS — precisamos config de overrides.
- **Tamanho do bundle aumenta** (mitigável, ambas são markdown + JS pequeno).
- **User avançado pode reclamar** ("quero gerar exatamente isso, sem audit") — aceitamos: target Atelier é quem QUER discipline, não quem quer fugir dela.

## Mitigações

- **Versão fixa das skills** em `package.json`, atualização requer ADR
- **Configuração de overrides** para anti-patterns impeccable que conflitam com DS Atelier (quando DS interno aparecer em M0.5+)
- **Modo "warning vs blocking":** anti-patterns críticos bloqueiam output, anti-patterns menores só warning inline (configurável por severidade, NÃO por skill desativada)
- **Documentar em manifesto + Director § obvio:** quem quer Atelier sem discipline, usa outra ferramenta

## Implementação

- Skills bundled em `packages/skills/impeccable/` e `packages/skills/taste-skill/` (mirror das upstream)
- Loader automático no startup do daemon Atelier
- Test obrigatório (TestSprite): bootar Atelier sem skills = erro de inicialização claro
- Sync mensal com upstreams (impeccable, taste-skill) — registrar em UPSTREAM.md se virar relevante

## Relação com outros ADRs
- [ADR-0001](ADR-0001-fork-open-design-vs-from-scratch.md): forkar open-design herda sistema de skills, mas Atelier ADICIONA obrigatoriedade Mora-specific
- [ADR-0004](ADR-0004-modes-studio-default-vereda-opt-in.md): skills só ativas em Studio (em Vereda Atelier não gera output, só explica)
- Futuro ADR sobre customization scope: como Strata ADR-0004, definirá linha entre "skill bundled travada" e "skills opt-in user-installable"
