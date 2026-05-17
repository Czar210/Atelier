# .speckit — Specs Vivas do Atelier

Pasta de **especificações ativas**. Conteúdo muda toda semana. Versionada no git porque a evolução das decisões é parte do registro.

## Hierarquia

```
manifesto.md           → POR QUE Atelier existe (imutável)
CONTEXT_DIRECTOR.md    → COMO trabalhamos (raramente muda)
.speckit/              → O QUE estamos construindo agora (vive)
CLAUDE.md              → instruções operacionais pra Claude Code
UPSTREAM.md            → sync tracking com open-design
```

## Estrutura

```
.speckit/
├── product/         specs de produto (visão, modos)
├── architecture/    decisões técnicas e ADRs
├── plans/           o que fazer agora (current), depois (backlog), feito (done)
└── tracking/        patch notes, bugs, decisões
```

## Regras

1. **Toda spec tem dono e data.** Header obrigatório: `dono:`, `atualizado:`, `status: [draft|ativo|congelado|obsoleto]`.
2. **Spec obsoleta não é deletada — é movida pra `_archive/` com motivo.** Histórico é evidência.
3. **ADRs são imutáveis após "ativo".** Mudança = novo ADR que supersedes o antigo.
4. **`current.md` reflete a iteração de hoje.** Quando ela termina, conteúdo migra pra `done.md` com data.
5. **Coordenar com Strata:** decisões que afetam ambos produtos (formato de nota Obsidian, etc.) precisam ser refletidas em ambos repos.

## Quando criar nova pasta

Só crie nova subpasta quando tiver **3+ arquivos** pra colocar. Pasta com um arquivo só = dívida cognitiva (regra Mora).
