# Case do Hackathon (4h) — “Crédito Sustentável no Campus”
## Contexto (história do negócio)
Uma cooperativa financeira universitária (“CoopCampus”) quer oferecer microcrédito para estudantes e recém-formados (ex.: notebook, curso, moradia, transporte).
O desafio: o orçamento é limitado e a inadimplência pode quebrar o programa. Ao mesmo tempo, negar crédito demais reduz impacto social e reputação.
A diretoria precisa decidir quem aprovar, qual limite e quais condições (ex.: parcelas, taxa, exigência de fiador), com base em dados.

## Problema central (o que deve ser decidido)
A CoopCampus precisa tomar uma decisão data-driven para o próximo ciclo de concessão (ex.: 2.000 solicitações). Ela quer:
1. Reduzir inadimplência esperada (risco)
2. Maximizar impacto/retorno (ex.: estudantes que de fato se formam, mantêm bom histórico, permanecem ativos)
3. Manter critérios minimamente justos e auditáveis (explicabilidade/traceabilidade)

Decisão final do time: recomendar uma política de concessão, por exemplo:
- Aprovar automaticamente quem tem score acima de X
- Revisão manual para faixa intermediária
- Negar abaixo de Y
- Definir limite/condição conforme risco (segmentação)

## Dados 
Estrutura das Tabelas e Schemas estão localizadas no arquivo `data_dictionary.csv`

## Dois caminhos (tracks) para os times
### Track 1 — Engenharia + Analytics (ponta a ponta)
#### Objetivo
Construir um fluxo de ingestão → limpeza → transformação → camada analítica e entregar insights acionáveis que fundamentem a política de concessão.
#### Entregáveis esperados
- Pipeline no Databricks
- Leitura dos CSVs
- Tratamento de nulos/outliers, normalização de datas, categorias
- Criação de tabelas “curadas” (ex.: Silver/Gold)
- Métricas e análises
- Perfil das solicitações (segmentos por curso, renda, finalidade)
- Taxa histórica de inadimplência por segmento (se houver histórico)
- Relação entre variáveis e risco: requested_amount, income, overdraft_events, months_employed, gpa, etc.
- Recomendação de decisão
Uma política clara do tipo:
- “Aprovar até R$X se renda > Y e overdraft_events < Z”
- “Exigir fiador em condição A”
- “Limite ajustado por faixa de risco”
#### Critérios de qualidade
- Dados: completude, consistência, duplicidade
- Auditoria: quais regras/transformações foram aplicadas
- Justificativa: por que isso melhora a decisão
#### Resultado esperado
Uma “camada Gold” com uma tabela final do tipo:
application_id, risk_segment, recommended_action, recommended_limit, rationale_features

### Track 2 — Modelo Preditivo (score de probabilidade)
#### Objetivo
Criar um modelo que gere um score de probabilidade de inadimplência (ou de bom pagador) para apoiar a decisão.

#### Entregáveis esperados
- Dataset de treino
- Join das tabelas por student_id
- Feature engineering (ex.: razão inflows/outflows, volatilidade de saldo, tendência do GPA)
- Modelo
- Algoritmo simples e rápido (ex.: Logistic Regression, RandomForest, Gradient Boosted Trees)
- Métrica mínima: AUC/ROC ou F1 (e matriz de confusão)
- Score + política
- Criar um score para cada aplicação
- Recomendar um threshold (X) e justificar trade-off:
- “Reduz inadimplência em Y% com queda de aprovação de Z%”
- Explicabilidade básica
- Importância de features ou coeficientes
- Quais fatores mais pesaram no risco
#### Resultado esperado
Uma tabela final com:
application_id, prob_default, risk_band (A/B/C), recommended_action

## Escopo (o que está dentro e fora)
### Dentro do escopo (deve ser possível em 4h)
- Ingestão de 3–5 arquivos (CSV) no Databricks
- Limpeza e joins
- Transformações + feature engineering
- Análise exploratória ou modelo baseline
- Recomendação objetiva de política de decisão
- Notebook executável + apresentação curta
## Fora do escopo
- Deploy/serving em produção
- MLOps completo (CI/CD, registry formal, monitoramento)
- Dashboards sofisticados (pode ser gráfico no notebook)
- Otimização pesada de performance

## Definição de “sucesso” (critérios de avaliação)
Sugestão de rubrica (100 pontos):
- Clareza do problema e decisão proposta (20)
- Qualidade do pipeline/dados (20)
- Qualidade analítica/modelo (25)
- Justificativa do trade-off (risco vs aprovação) (20)
- Comunicação e reprodutibilidade (15)
(notebook roda do zero, leitura clara, conclusão objetiva)

## Plano de execução (4 horas)
- 0:00–0:20 — Briefing + escolha do Track + entendimento dos dados
- 0:20–1:40 — Ingestão, limpeza, joins e tabela base
- 1:40–2:50 — Análises/feature engineering + modelo (se Track 2)
- 2:50–3:30 — Recomendações e “política” final
- 3:30–4:00 — Preparar pitch + exportar resultados

## Observações (para o desafio de limpeza/ingestão)
Mesmo “padrão”, os alunos vão enfrentar:
- Deduplicação (students, applications, transactions_monthly)
- Agregação correta de transações (múltiplas linhas por estudante/mês)
- Tratamento de série temporal incompleta (estudantes com poucos meses)
- Joins e chaves (student_id e loan/application)


