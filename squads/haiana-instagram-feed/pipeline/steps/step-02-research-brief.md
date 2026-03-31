---
execution: subagent
agent: rita-referencia
inputFile: squads/haiana-instagram-feed/output/research-focus.md
outputFile: squads/haiana-instagram-feed/output/research-brief-generated.md
model_tier: powerful
---

# Step 02: Research Brief

## Context Loading

Load these files before executing:
- `squads/haiana-instagram-feed/output/research-focus.md` - tema e janela temporal escolhidos pelo usuário
- `squads/haiana-instagram-feed/pipeline/data/research-brief.md` - base fixa de pesquisa e posicionamento
- `squads/haiana-instagram-feed/pipeline/data/quality-criteria.md` - critérios de segurança e clareza
- `squads/haiana-instagram-feed/pipeline/data/branding-notes.md` - contexto de marca

## Instructions

### Process

1. Entenda a pergunta central do tema do usuário e defina o que o post deve resolver.
2. Pesquise fontes atuais e confiáveis quando o tema exigir atualização temporal.
3. Produza um brief com achados, mitos, linguagem segura, CTA sugerido e riscos de interpretação.

## Output Format

The output MUST follow this exact structure:

```markdown
# Research Brief Generated

## Topic

## Audience Question

## Key Findings

## Plain-Language Translation

## Risks And Cautions

## Recommended Post Direction

## Suggested CTA

## Sources
```

## Output Example

```markdown
# Research Brief Generated

## Topic
5 sinais de que vale investigar risco para diabetes tipo 2

## Audience Question
Como perceber se vale procurar avaliação?

## Key Findings
- excesso de peso e sedentarismo aumentam risco
- histórico familiar pesa
- pré-diabetes merece atenção precoce

## Plain-Language Translation
- use "vale investigar" em vez de "você provavelmente tem"
- use "fatores de risco" em vez de linguagem fatalista

## Risks And Cautions
- não transformar sinais em diagnóstico
- não prometer prevenção absoluta

## Recommended Post Direction
Lista educativa com fechamento preventivo

## Suggested CTA
Salve para rever antes dos próximos exames.

## Sources
- CDC ...
```

## Veto Conditions

Reject and redo if ANY are true:
1. O brief não citar fontes.
2. O texto confundir educação com prescrição.

## Quality Criteria

- [ ] Tema entendido
- [ ] Fontes confiáveis
- [ ] Linguagem simples
- [ ] Riscos explicitados
