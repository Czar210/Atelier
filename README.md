<div align="center">

# Atelier

*"Designs que você decide. Não que decidem por você."*

[![License: Apache-2.0](https://img.shields.io/badge/License-Apache_2.0-c084fc?style=flat-square&logo=opensourceinitiative&logoColor=white)](LICENSE)
[![Mora Org](https://img.shields.io/badge/Mora-Org-5eead4?style=flat-square)](https://github.com/Mora-Org)
[![Fork of open-design](https://img.shields.io/badge/Fork-open--design-fb7185?style=flat-square)](https://github.com/nexu-io/open-design)

</div>

---

## O que é

Atelier é um **gerador de design local-first** que coloca brand discipline na frente do output rápido. Fork/upgrade Mora-flavored do [open-design](https://github.com/nexu-io/open-design) (Apache-2.0).

Por padrão entra em criação — você descreve o que quer, Atelier conversa, propõe, ajusta, exporta prototypes / slides / images / videos. Bundled e obrigatório: **impeccable** (audit) + **taste-skill** (taste consistency). Não são opcionais — discipline embarcada.

Quando você quer entender em vez de produzir, ativa **Modo Vereda** — Atelier explica princípios, aponta referências primárias e deposita nota no seu vault Obsidian (opcional, não vital).

Roda local. Funciona offline.

---

## Dois modos

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  [ STUDIO ]  Padrão                                             │
│              Cria. Gera. Audita. Polia.                         │
│              DS + prototypes + slides + image + video.          │
│              impeccable + taste-skill sempre carregados.        │
│                                                                 │
│  [ VEREDA ]  Opt-in explícito                                   │
│              Ensina. Explica princípios de design.              │
│              Referências primárias (Müller-Brockmann, Lupton…). │
│              Gera nota Obsidian se você tem vault.              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

Invertido do [Strata](https://github.com/Mora-Org/strata) — lá Vereda é padrão e Mestre é opt-in. Faz sentido: **design começa fazendo, código começa entendendo.**

---

## Como funciona

1. Você traz um brief ou um brand system
2. **Modo Studio:** Atelier conversa, gera prototype/slide/image, audita com impeccable, ajusta taste com taste-skill, exporta
3. **Modo Vereda:** Atelier explica princípio, cita referência primária, deposita nota
4. **Você decide.** O design é seu.

---

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Core | Fork/upgrade do [open-design](https://github.com/nexu-io/open-design) (Apache-2.0) |
| Agent delegation | Pi (`@mariozechner/pi-coding-agent`) ou outros via PATH scan herdado do upstream |
| GUI | Tauri 2 + React + TypeScript |
| Skills embarcadas | impeccable (audit) + taste-skill (consistency) |
| Modelos locais | Ollama (padrão) — cloud opt-in |
| Vault (opcional) | Obsidian (markdown + frontmatter) |

---

## Por que existe

Design tools modernas otimizam pra um destes extremos: página em branco (Figma) ou template infinito (galerias). Atelier ocupa o meio com discipline embarcada — você cria rápido, mas não cria slop.

A maioria das ferramentas IA promete *"faça por mim"*. Atelier propõe *"decida comigo"*.

Leia o [**Manifesto Atelier →**](manifesto.md)

---

## Fork attribution

Atelier é fork Mora-flavored do [open-design](https://github.com/nexu-io/open-design) por nexu-io, licenciado Apache-2.0. README original preservado em [`README.upstream.md`](README.upstream.md). Tracking de sync em [`UPSTREAM.md`](UPSTREAM.md).

Mudanças Mora documentadas em [`.speckit/`](.speckit/) e [`CONTEXT_DIRECTOR.md`](CONTEXT_DIRECTOR.md).

---

## Parte da Mora

> *Glyph · Atlas · Lattes Director · [Strata](https://github.com/Mora-Org/strata) · **Atelier***

[**Mora Org →**](https://github.com/Mora-Org)

---

<div align="center">

**Atelier** · Um produto Mora · Fork open-source com alma

</div>
