---
task: "Otimizar legenda e CTA"
order: 2
input: |
  - slides: estrutura do carrossel
  - visual_direction: direção visual
output: |
  - caption: legenda final
  - hashtags: hashtags finais
  - final_package: pacote completo consolidado
---

# Otimizar legenda e CTA

Feche o pacote com legenda, hashtags e instruções finais de uso.

## Process

1. Escreva uma legenda com hook forte, corpo claro e fechamento útil.
2. Gere hashtags compatíveis com o nicho e com o tema.
3. Consolide o pacote completo para revisão.

## Output Format

```yaml
caption: "..."
hashtags:
  - "#..."
final_package:
  delivery: "complete"
```

## Output Example

```yaml
caption: "Prevenir começa quando você reconhece os sinais certos..."
hashtags:
  - "#diabetes"
  - "#obesidade"
  - "#endocrinologia"
final_package:
  delivery: "complete"
```

## Quality Criteria

- [ ] Hook nos primeiros 125 caracteres
- [ ] CTA claro
- [ ] Hashtags relevantes

## Veto Conditions

Reject and redo if ANY are true:
1. A legenda parecer repetição do carrossel sem ganho.
2. As hashtags forem genéricas demais ou exageradas.
