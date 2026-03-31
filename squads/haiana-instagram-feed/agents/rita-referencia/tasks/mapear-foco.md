---
task: "Mapear foco"
order: 1
input: |
  - topic: tema informado pelo usuário
  - time_range: janela temporal escolhida no checkpoint
output: |
  - editorial_question: pergunta central do post
  - search_plan: fontes e ângulos de busca
---

# Mapear foco

Transforme o tema do usuário em uma pergunta editorial acionável e em um plano de pesquisa enxuto.

## Process

1. Identifique a dor, dúvida ou mito principal por trás do tema.
2. Converta o tema em uma pergunta que o público geral faria.
3. Liste quais tipos de fonte devem ser buscados e quais riscos merecem atenção.

## Output Format

```yaml
editorial_question: "..."
search_plan:
  - source_type: "..."
    reason: "..."
risks:
  - "..."
```

## Output Example

```yaml
editorial_question: "Quais sinais indicam que vale investigar risco para diabetes tipo 2?"
search_plan:
  - source_type: "órgão oficial"
    reason: "validar fatores de risco e linguagem segura"
  - source_type: "guia de comunicação em saúde"
    reason: "traduzir o assunto para leigos"
risks:
  - "transformar rastreio em diagnóstico"
  - "soar alarmista"
```

## Quality Criteria

- [ ] Pergunta editorial específica
- [ ] Plano de busca objetivo
- [ ] Riscos já antecipados

## Veto Conditions

Reject and redo if ANY are true:
1. O output continuar genérico demais para virar pesquisa.
2. Não houver risco editorial ou clínico identificado.
