---
execution: subagent
agent: vera-veredicto
inputFile: squads/haiana-instagram-feed/output/content-package.md
outputFile: squads/haiana-instagram-feed/output/review-report.md
model_tier: powerful
on_reject: step-05-create-feed
---

# Step 06: Review Content

## Context Loading

Load these files before executing:
- `squads/haiana-instagram-feed/output/content-package.md` - pacote de conteúdo gerado
- `squads/haiana-instagram-feed/pipeline/data/quality-criteria.md` - critérios formais
- `squads/haiana-instagram-feed/pipeline/data/branding-notes.md` - referência visual
- `squads/haiana-instagram-feed/pipeline/data/anti-patterns.md` - erros a evitar

## Instructions

### Process

1. Avalie clareza, precisão, adequação ao Instagram, branding e CTA.
2. Gere notas com justificativa.
3. Entregue veredito claro e caminho de correção, se necessário.

## Output Format

The output MUST follow this exact structure:

```markdown
# Review Report

## Verdict

## Scoring Table

## Strengths

## Required Changes

## Non-Blocking Suggestions

## Summary
```

## Output Example

```markdown
# Review Report

## Verdict
CONDITIONAL APPROVE

## Scoring Table
- Clareza: 8/10 porque ...
- Precisão clínica: 8/10 porque ...

## Strengths
- capa clara
- legenda útil

## Required Changes
- deixar o slide 6 menos ambíguo sobre individualização

## Non-Blocking Suggestions
- reduzir uma linha na legenda

## Summary
Peça sólida, com ajuste leve antes de uso.
```

## Veto Conditions

Reject and redo if ANY are true:
1. O review não trouxer veredito inequívoco.
2. Houver crítica sem apontar correção.

## Quality Criteria

- [ ] Notas justificadas
- [ ] Veredito coerente
- [ ] Ajustes obrigatórios separados
- [ ] Forças reconhecidas
