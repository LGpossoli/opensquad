---
task: "Gerar feedback"
order: 2
input: |
  - scoring_table: notas por critério
  - blocking_risks: riscos bloqueantes
output: |
  - verdict: veredito final
  - required_changes: ajustes obrigatórios
  - suggestions: sugestões não bloqueantes
---

# Gerar feedback

Converta a pontuação em um parecer final claro e acionável.

## Process

1. Determine o veredito conforme notas e riscos.
2. Liste mudanças obrigatórias com localização e correção.
3. Liste sugestões opcionais.

## Output Format

```yaml
verdict: "APPROVE"
required_changes:
  - "..."
suggestions:
  - "..."
```

## Output Example

```yaml
verdict: "CONDITIONAL APPROVE"
required_changes:
  - "deixar mais claro no slide 6 que o post não substitui avaliação"
suggestions:
  - "reduzir uma linha no slide 3"
```

## Quality Criteria

- [ ] Veredito claro
- [ ] Ajustes acionáveis
- [ ] Sugestões separadas

## Veto Conditions

Reject and redo if ANY are true:
1. O veredito contradizer as notas.
2. Houver rejeição sem caminho de correção.
