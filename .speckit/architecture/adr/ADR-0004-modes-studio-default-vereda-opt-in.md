---
dono: Cesar
atualizado: 2026-05-17
status: ativo
supersedes: nenhum
superseded_by: nenhum
---

# ADR-0004 — Modes: Studio default + Vereda opt-in (invertido vs Strata)

## Contexto

Mora estabeleceu **dois modos** como conceito transversal de produto (manifesto Strata §IV/V):

- **Vereda:** ensino, explica, gera nota, não produz output executável/aplicável
- **Modo executivo:** faz o trabalho (no Strata = Mestre coda; no Atelier = ?)

No Strata, **Vereda é padrão** e Mestre é opt-in. Razão: código aprendido é código defendível.

No Atelier, o domínio é diferente. Pergunta: invertemos defaults ou mantemos paradigma Strata?

## Decisão

**Invertemos.** No Atelier:

- **Studio (executivo)** é o padrão — toda nova sessão abre em Studio
- **Vereda (ensino)** é opt-in com fricção (`/vereda` + confirmação)

## Justificativa

### Design começa fazendo
Reflexão antes de protótipo gera page-blank paralysis. Design matures por iteração rápida — você produz 5 versões e escolhe. Studio default permite esse ritmo.

### Código começa entendendo
Código mal-entendido vira dívida técnica. Reflexão antes de mudança gera código defendível. Vereda default no Strata permite essa pausa.

### Dois produtos, dois ritmos respeitados
Cada produto Mora respeita o ritmo natural do seu domínio. Forçar paradigma idêntico nos dois = desrespeitar diferença real.

### Vereda continua existindo
Vereda no Atelier não é decoração — é tool real pra quando user quer entender (porque está bloqueado, porque quer ensinar alguém, porque está revisitando decisão). Opt-in com fricção mantém intenção deliberada.

## Consequências

### Positivas
- Atelier é imediatamente útil — user abre, pede prototype, recebe
- Vereda como opt-in mantém compromisso Mora com ensino sem forçar
- Paradigma "dois modos Mora" preservado, defaults adaptados ao domínio

### Negativas
- **Inconsistência de paradigma:** user que conhece Strata tem que aprender que Atelier é o oposto. Risco de surpresa.
- **Toggle ambíguo:** se user esquece em qual modo está, comportamento difere. Mitigação: indicador visual permanente (design pendente).
- **Vereda menos usado:** se default é Studio, Vereda vira opt-in raro. Risco de virar feature morta se não tiver fricção curada bem.

## Mitigações

- **Documentação clara:** README + manifesto explicitam inversão e por quê
- **Indicador visual:** mode badge sempre visível (similar ao Strata mas com cores adequadas — design em M0.5+)
- **Friction balanceada:** Studio→Vereda precisa confirmação, mas menor que Strata Vereda→Mestre (lá é tema de segurança; aqui é tema cultural)
- **Métricas:** trackar razão Studio/Vereda. Se Vereda < 5% do uso depois de 6 meses, revisitar (talvez vire feature morta — não temos esse problema no Strata por design).

## Casos de teste obrigatórios (TestSprite)

- Sessão nova abre em Studio — sempre
- `/vereda` sem confirmação não ativa
- Em Vereda, tentativa de export é bloqueada com mensagem clara
- Fechar e reabrir = Studio (mesmo se última sessão era Vereda)
- impeccable e taste-skill rodam só em Studio (não em Vereda)

## Relação com outros ADRs
- [ADR-0001](ADR-0001-fork-open-design-vs-from-scratch.md): fork open-design herda agent delegation pattern, modos são layer Mora sobre isso
- [ADR-0002](ADR-0002-local-first-tauri-stack.md): Tauri stack é independente; modos rodam em qualquer wrapper
- [ADR-0003](ADR-0003-bundle-impeccable-and-taste-skill.md): skills bundled só ativas em Studio
- **Strata ADR equivalente:** Strata `.speckit/product/modes-spec.md` (Vereda padrão + Mestre opt-in) — Atelier explicitamente diverge nessa decisão
