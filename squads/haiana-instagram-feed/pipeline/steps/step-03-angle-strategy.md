---
execution: inline
agent: sofia-sintese
inputFile: squads/haiana-instagram-feed/output/research-brief-generated.md
outputFile: squads/haiana-instagram-feed/output/angle-options.md
---

# Step 03: Angle Strategy

## Context Loading

Load these files before executing:
- `squads/haiana-instagram-feed/output/research-brief-generated.md` - brief pesquisado do tema atual
- `squads/haiana-instagram-feed/pipeline/data/domain-framework.md` - metodologia do squad
- `squads/haiana-instagram-feed/pipeline/data/tone-of-voice.md` - opções de tom
- `squads/haiana-instagram-feed/pipeline/data/branding-notes.md` - direção de marca

## Instructions

### Process

1. Escolha o pilar editorial e o formato do carrossel mais adequado.
2. Gere 3 ângulos realmente diferentes para o mesmo tema.
3. Em cada ângulo, entregue hook, promessa, tom sugerido e CTA.

## Output Format

The output MUST follow this exact structure:

```markdown
# Angle Options

## Recommended Format

## Recommended Pillar

## Option 1
- Angle:
- Hook:
- Promise:
- Tone:
- CTA:

## Option 2
- Angle:
- Hook:
- Promise:
- Tone:
- CTA:

## Option 3
- Angle:
- Hook:
- Promise:
- Tone:
- CTA:

## Recommendation
```

## Output Example

```markdown
# Angle Options

## Recommended Format
Lista

## Recommended Pillar
Educação preventiva

## Option 1
- Angle: checklist prático
- Hook: 5 sinais de que vale investigar risco para diabetes tipo 2
- Promise: ajudar o leitor a perceber quando procurar avaliação
- Tone: educativo direto
- CTA: salve para rever depois

## Option 2
- Angle: erro comum
- Hook: você não precisa esperar sintomas gritantes para olhar o risco
- Promise: corrigir a ideia de que diabetes sempre dá sinais óbvios
- Tone: autoridade serena
- CTA: envie para alguém que adia exame

## Option 3
- Angle: mito vs realidade
- Hook: diabetes tipo 2 não começa apenas quando a glicose "explode"
- Promise: mostrar o que pode aparecer antes
- Tone: mito vs realidade
- CTA: comente sua dúvida

## Recommendation
Option 1 porque é mais salvável para feed.
```

## Veto Conditions

Reject and redo if ANY are true:
1. As três opções forem parecidas.
2. Faltar recomendação clara.

## Quality Criteria

- [ ] Formato recomendado
- [ ] Pilar recomendado
- [ ] Três opções contrastantes
- [ ] Recomendação final
