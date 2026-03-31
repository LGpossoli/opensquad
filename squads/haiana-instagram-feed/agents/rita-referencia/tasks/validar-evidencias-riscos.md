---
task: "Validar evidências e riscos"
order: 2
input: |
  - editorial_question: pergunta central
  - search_plan: plano definido na etapa anterior
output: |
  - key_findings: achados práticos
  - safe_language: linguagem recomendada
  - caution_notes: alertas clínicos e comunicacionais
---

# Validar evidências e riscos

Pesquise, valide e resuma o que o squad precisa saber para criar um post confiável.

## Process

1. Busque fontes adequadas ao plano e selecione as mais confiáveis.
2. Resuma fatos, linguagem segura e pontos de cuidado.
3. Entregue um brief pronto para criação de pauta.

## Output Format

```yaml
key_findings:
  - finding: "..."
    source: "..."
safe_language:
  - "..."
caution_notes:
  - "..."
cta_suggestion: "..."
```

## Output Example

```yaml
key_findings:
  - finding: "Excesso de peso e sedentarismo aumentam risco para diabetes tipo 2."
    source: "CDC"
  - finding: "Conteúdo para leigos deve priorizar plain language."
    source: "CDC Health Literacy"
safe_language:
  - "fatores de risco"
  - "vale investigar"
  - "procure avaliação"
caution_notes:
  - "não tratar lista de sinais como diagnóstico"
  - "evitar tom de culpa"
cta_suggestion: "salve este post para rever depois"
```

## Quality Criteria

- [ ] Fontes aparecem
- [ ] Achados são acionáveis
- [ ] Notas de cautela são claras

## Veto Conditions

Reject and redo if ANY are true:
1. O texto recomendar conduta individual como se fosse consulta.
2. Não houver fonte ou ponto de cautela.
