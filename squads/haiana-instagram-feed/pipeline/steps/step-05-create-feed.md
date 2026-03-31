---
execution: inline
agent: iris-instagram
format: instagram-feed
inputFile: squads/haiana-instagram-feed/output/angle-options.md
outputFile: squads/haiana-instagram-feed/output/content-package.md
---

# Step 05: Create Feed

## Context Loading

Load these files before executing:
- `squads/haiana-instagram-feed/output/angle-options.md` - opções de ângulo
- `squads/haiana-instagram-feed/pipeline/data/research-brief.md` - base fixa do squad
- `squads/haiana-instagram-feed/output/research-brief-generated.md` - pesquisa do tema atual
- `squads/haiana-instagram-feed/pipeline/data/branding-notes.md` - branding obrigatório
- `squads/haiana-instagram-feed/pipeline/data/tone-of-voice.md` - tons disponíveis
- `squads/haiana-instagram-feed/pipeline/data/quality-criteria.md` - critérios finais

## Instructions

### Process

1. Use a escolha feita no checkpoint imediatamente anterior para definir ângulo e tom.
2. Crie um pacote completo de Instagram Feed com slides, legenda, CTA, hashtags e direção visual.
3. Certifique-se de que o resultado seja útil, clínico na medida certa e alinhado ao branding.

## Output Format

The output MUST follow this exact structure:

```markdown
# Content Package

## Chosen Angle

## Format

## Slides

## Caption

## CTA

## Hashtags

## Visual Direction

## Clinical Caution Note
```

## Output Example

```markdown
# Content Package

## Chosen Angle
Checklist preventivo

## Format
Lista

## Slides
Slide 1: ...
Slide 2: ...

## Caption
Prevenir começa quando você reconhece os sinais certos...

## CTA
Salve este post para rever depois.

## Hashtags
#diabetes #obesidade #endocrinologia

## Visual Direction
- Headline font: Playfair Display Medium
- Body font: Lato Light
- Palette: #B0947C, #502D17, #FFFFFF

## Clinical Caution Note
Este conteúdo é educativo e não substitui avaliação individual.
```

## Veto Conditions

Reject and redo if ANY are true:
1. O pacote não trouxer direção visual específica.
2. A peça fizer promessa clínica inadequada.

## Quality Criteria

- [ ] Pacote completo
- [ ] Branding aplicado
- [ ] CTA claro
- [ ] Nota de cautela clínica
