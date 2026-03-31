---
task: "Definir pilar e formato"
order: 1
input: |
  - research_brief: resumo pesquisado
output: |
  - pillar: pilar editorial
  - format_choice: formato de carrossel
  - rationale: justificativa
---

# Definir pilar e formato

Escolha o pilar e o formato do carrossel com base na utilidade do tema para o público.

## Process

1. Identifique qual valor o post entrega: prevenção, esclarecimento, adesão ou correção de mito.
2. Escolha o pilar editorial mais forte.
3. Defina o formato de carrossel com justificativa objetiva.

## Output Format

```yaml
pillar: "..."
format_choice: "..."
rationale: "..."
```

## Output Example

```yaml
pillar: "educação preventiva"
format_choice: "lista"
rationale: "o tema pede consulta posterior e checkpoints claros"
```

## Quality Criteria

- [ ] Pilar claro
- [ ] Formato claro
- [ ] Justificativa prática

## Veto Conditions

Reject and redo if ANY are true:
1. Formato escolhido não combina com o tema.
2. A justificativa é genérica.
