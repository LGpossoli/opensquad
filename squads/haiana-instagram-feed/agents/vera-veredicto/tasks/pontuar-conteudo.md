---
task: "Pontuar conteúdo"
order: 1
input: |
  - content_package: pacote final criado
  - quality_criteria: critérios de avaliação
output: |
  - scoring_table: notas por critério
  - blocking_risks: riscos bloqueantes
---

# Pontuar conteúdo

Atribua notas por critério com justificativa curta.

## Process

1. Avalie cada dimensão principal.
2. Registre riscos bloqueantes, se houver.
3. Prepare base para o veredito final.

## Output Format

```yaml
scoring_table:
  - criterion: "..."
    score: "8/10"
    reason: "..."
blocking_risks:
  - "..."
```

## Output Example

```yaml
scoring_table:
  - criterion: "clareza"
    score: "8/10"
    reason: "slides bem organizados e linguagem simples"
blocking_risks:
  - "nenhum"
```

## Quality Criteria

- [ ] Critérios cobertos
- [ ] Justificativa em cada nota
- [ ] Riscos explícitos

## Veto Conditions

Reject and redo if ANY are true:
1. Houver nota sem justificativa.
2. Um risco clínico grave não estiver marcado.
