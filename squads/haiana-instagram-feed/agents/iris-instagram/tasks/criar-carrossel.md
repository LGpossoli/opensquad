---
task: "Criar carrossel"
order: 1
input: |
  - selected_angle: ângulo selecionado no checkpoint
  - research_brief: contexto factual
output: |
  - slides: estrutura completa do carrossel
  - visual_direction: direção visual
---

# Criar carrossel

Monte a espinha dorsal da peça com slides completos e direção visual.

## Process

1. Escolha o formato do carrossel com base no ângulo selecionado.
2. Escreva a capa e os slides em ordem lógica.
3. Defina estilo visual coerente com o branding.

## Output Format

```yaml
format: "..."
slides:
  - slide: 1
    role: "cover"
    title: "..."
    body: "..."
visual_direction:
  palette:
    - "#..."
```

## Output Example

```yaml
format: "lista"
slides:
  - slide: 1
    role: "cover"
    title: "5 sinais de que vale investigar risco para diabetes tipo 2"
    body: "Capa com promessa clara e leitura imediata."
visual_direction:
  palette:
    - "#B0947C"
    - "#502D17"
```

## Quality Criteria

- [ ] Capa forte
- [ ] Ordem lógica
- [ ] Direção visual específica

## Veto Conditions

Reject and redo if ANY are true:
1. Slides não formarem sequência clara.
2. Direção visual não mencionar o branding.
