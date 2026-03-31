---
task: "Gerar ângulos e hooks"
order: 2
input: |
  - pillar: pilar escolhido
  - format_choice: formato escolhido
output: |
  - angle_options: três ângulos com hook, promessa, tom e CTA
---

# Gerar ângulos e hooks

Crie três caminhos realmente diferentes para o mesmo tema.

## Process

1. Gere três perspectivas distintas.
2. Dê a cada ângulo um hook e uma promessa.
3. Sugira tom e CTA de baixo atrito.

## Output Format

```yaml
angle_options:
  - angle: "..."
    hook: "..."
    promise: "..."
    tone: "..."
    cta: "..."
```

## Output Example

```yaml
angle_options:
  - angle: "checklist preventivo"
    hook: "5 sinais de que vale investigar risco para diabetes tipo 2"
    promise: "ajudar o leitor a reconhecer quando procurar avaliação"
    tone: "educativo direto"
    cta: "salve para rever nos exames"
```

## Quality Criteria

- [ ] Três opções distintas
- [ ] Hooks claros
- [ ] Promessa útil

## Veto Conditions

Reject and redo if ANY are true:
1. Dois ângulos parecerem iguais.
2. Hook sem clareza ou utilidade.
