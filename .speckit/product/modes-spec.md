---
dono: Cesar
atualizado: 2026-05-17
status: draft
---

# Spec dos Dois Modos (Atelier)

**Invertido vs Strata:** Studio (criação) é padrão, Vereda (ensino) é opt-in. Justificativa: design começa fazendo, código começa entendendo.

## Modo Studio (padrão)

### Pode
- Gerar design systems novos (paleta, tipografia, spacing, primitivos)
- Gerar prototypes (web, desktop, mobile)
- Gerar slides, images, videos
- Aceitar brand system existente e estender
- Exportar pra HTML/PDF/PPTX/MP4/ZIP (herdado do upstream)
- Conversar pra ajustar (chat-based iteration)
- Sandboxed preview do output

### Sempre carrega (bundled, não opcional)
- **impeccable** — audita todo output ANTES de mostrar ao user. Se detect retorna anti-patterns, mostra warning inline.
- **taste-skill** — configura dial inicial baseado em DS escolhido. Coerência inter-artefatos validada a cada novo asset.

### Não pode
- Pular impeccable audit ("turn off discipline")
- Pular taste-skill consistency check
- Editar vault existente do usuário (se configurado, só cria em `inbox/`)
- Subir arquivos pra cloud sem opt-in explícito do user

### Output esperado de um turno
1. Resposta no chat (proposta de design)
2. Preview sandboxed (iframe com HTML/CSS ou imagem renderizada)
3. impeccable audit inline (se warnings, mostra; se clean, ok silencioso)
4. taste-skill check inline (idem)
5. Botão "Export" disponível (formatos: HTML/PDF/PPTX/MP4/ZIP)
6. **Sem nota Obsidian gerada** (esse comportamento é só de Vereda)

## Modo Vereda (opt-in)

### Ativação
- Comando explícito do usuário (ex: `/vereda`, botão com confirmação modal)
- **Não persiste entre sessões.** Toda nova sessão começa em Studio.
- Indicador visual permanente quando ativo (badge, cor diferente, etc. — design pendente)

### Pode
- Explicar princípios de design (hierarquia, contraste, ritmo, voz)
- Citar referências primárias (Müller-Brockmann, Lupton, Bringhurst, Frutiger, Tschichold, Lupton again, Rand, Rams)
- Gerar nota Obsidian se vault configurado (formato canônico igual Strata)
- Apontar arquivos de design existentes no projeto pra refletir
- Mostrar pseudo-design didático curto (mockup conceitual, não pronto pra usar)

### Não pode
- Gerar prototype completo pronto pra exportar
- Continuar ativo após `/sair-vereda` ou fim da sessão
- Editar arquivos existentes do projeto sem permissão

### Output esperado de um turno
1. Resposta no chat (explicação + referências)
2. **Quando há ação no design**, resposta termina com **"ponte pro Studio"**: "agora ative Studio e implemente X com Y constraint"
3. Nota `.md` em `inbox/` (se vault configurado) com frontmatter padrão Mora:
   - `tags: [atelier, design, {area}]`
   - `bloom: {1-6}` (Bloom revisado aplicado a design)
   - `data: {YYYY-MM-DD}`
   - `refs: [{ref primária}]`
   - `relacionados: [[{nota anterior}]]`
4. Nenhum side-effect além disso (sem prototype, sem export, sem edição)

## Fluxo de transição

```
[Studio] --usuário digita /vereda--> [Confirmação] --sim--> [Vereda]
                                          |
                                          não
                                          v
                                       [Studio]

[Vereda] --usuário digita /studio OU fecha sessão--> [Studio]
```

Fricção menor que Strata (lá Mestre opt-in é cara): aqui Vereda também tem fricção, mas Studio→Vereda é mais cultural ("estou parando pra entender") que de segurança ("estou liberando capacidade destrutiva").

## Restrição de design (modelo local)

Mesma lição do Strata: prompts gigantes quebram modelo local 7B. Modo Vereda no Atelier também fica em **prompt ≤2K tokens** (alvo). Tunar em iteração futura.

## Casos de teste obrigatórios (TestSprite)

1. Sessão nova abre em Studio — sempre.
2. `/vereda` sem confirmação não ativa.
3. Em Vereda, tentativa de export é bloqueada com mensagem clara ("Vereda explica — para gerar, ative Studio").
4. Em Studio, impeccable roda antes de mostrar output (mesmo se passa clean).
5. Em Studio, taste-skill check aparece inline em todo turno.
6. Fechar e reabrir = volta pra Studio mesmo se última sessão era Vereda.
7. Vault não-configurado: Atelier funciona normalmente, comando `/vereda` opera sem gerar nota (só explica no chat).
8. impeccable desabilitado via hack: app emite warning grande e bloqueia output (não permite "discipline-off mode").
