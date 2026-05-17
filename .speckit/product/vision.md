---
dono: Cesar
atualizado: 2026-05-17
status: ativo
---

# Visão de Produto — Atelier

Resumo executivo do `manifesto.md` em formato operacional. Para a versão poética e completa, leia o manifesto.

## Problema

Design tools modernas otimizam para um destes extremos:
- **Página em branco** (Figma, Sketch): toda decisão pesa antes de qualquer experimento. Fricção alta para iteração rápida.
- **Template infinito** (galerias de UI kits): toda decisão já foi tomada por outro. Output genérico, autoria zero.

A IA generativa promete o meio, mas a maioria das ferramentas se posiciona como "faça por mim" — output rápido sem discipline. Resultado: portfólios de coisas que ninguém defende.

## Proposta

**Gerador de design local-first com discipline embarcada.** Atelier conversa, propõe, ajusta, exporta — mas com:

- **impeccable** auditando todo output (27 anti-patterns deterministicos rodando antes de mostrar ao user)
- **taste-skill** mantendo coerência inter-artefatos (você não consegue gerar um landing brutalist + uma slide minimalist no mesmo projeto sem ser avisado)
- **Studio mode (default):** cria. Gera DS, prototypes, slides, images, videos.
- **Vereda mode (opt-in):** ensina. Explica princípios, cita Müller-Brockmann/Lupton/Bringhurst, deposita nota.

Roda local (fork do open-design Apache-2.0). Vault Obsidian opcional. Cloud opt-in.

## Métrica de sucesso

Não é "designs gerados por semana". É:
- **Quantos design systems próprios** o user mantém ativos (não consumindo templates)
- **Razão criar/iterar alta** — user volta a uma DS pra ajustar, em vez de jogar fora e começar nova
- **Razão Studio/Vereda alta (>5:1)** — user usa Vereda como exceção pra entender, não como regra
- **% de output que passa impeccable sem warning** crescendo no tempo (user internalizando discipline)

## Anti-objetivos explícitos

- **Não competir com Figma em colaboração multi-user.** Atelier é local, single-user (multi-user é roadmap futuro via sync opcional).
- **Não ser AI image generator "façam por mim".** Atelier propõe, user decide.
- **Não substituir Adobe Suite** (Photoshop, Illustrator, etc.) — Atelier é generative + brand discipline, não editor pixel-level.
- **Não bloquear export.** Exports vão pro disco do user em formato aberto. Sem lock-in.
- **Não esconder Modo Vereda nem puni-lo** — só não torná-lo o caminho natural.
- **Não logar conteúdo gerado** (privacidade do brand do user é absoluta).
- **Não exigir vault Obsidian** — Atelier funciona sem. Vault é opcional, não vital.
