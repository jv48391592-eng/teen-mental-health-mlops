# Relatório Técnico

**Pipeline MLOps e Classificação com Rede Neural MLP**

## 1. Introdução

O presente trabalho visa desenvolver um modelo de classificação binária para prever a presença de depressão em adolescentes (`depression_label`) com base em hábitos digitais e fatores de saúde mental. O dataset utilizado contém informações sobre idade, gênero, horas diárias em redes sociais, horas de sono, níveis de estresse, ansiedade, entre outras variáveis.

O problema é formulado como uma tarefa de **classificação binária**, onde o objetivo é identificar precocemente adolescentes com maior risco de depressão, auxiliando em intervenções preventivas.

## 2. Metodologia

### 2.1 Pré-processamento e Limpeza dos Dados
- Remoção de duplicatas.
- Imputação de valores ausentes: **mediana** para variáveis numéricas (robusta a outliers) e valor constante "missing" para variáveis categóricas.
- Codificação da variável alvo com `LabelEncoder`.
- One-Hot Encoding nas variáveis categóricas.

### 2.2 Seleção de Variáveis

Foram utilizados três métodos distintos de feature selection, combinados em um ranking final normalizado:

**Tabela 1: Ranking de Features**

| Feature                    | Pearson   | Mutual Info | RF Importance | Final Score |
|---------------------------|-----------|-------------|---------------|-------------|
| sleep_hours               | 1.0000    | 0.8969      | 1.0000        | **0.9656**  |
| daily_social_media_hours  | 0.9191    | 0.8765      | 0.9618        | **0.9191**  |
| stress_level              | 0.8943    | 1.0000      | 0.7748        | **0.8897**  |
| anxiety_level             | 0.8895    | 0.8279      | 0.8259        | **0.8478**  |
| addiction_level           | 0.0732    | 0.0879      | 0.2197        | 0.1269      |
| screen_time_before_sleep  | 0.0866    | 0.0000      | 0.2745        | 0.1204      |
| academic_performance      | 0.0076    | 0.0000      | 0.2966        | 0.1014      |
| physical_activity         | 0.0923    | 0.0000      | 0.2012        | 0.0979      |
| age                       | 0.0576    | 0.0000      | 0.1443        | 0.0673      |

**Features selecionadas** (threshold > 0.3):  
`sleep_hours`, `daily_social_media_hours`, `stress_level`, `anxiety_level`.

### 2.3 Arquitetura da MLP e Hiperparâmetros

Foi implementada uma rede MLP com duas camadas ocultas, utilizando:
- Função de ativação: **ReLU**
- Regularização: **Dropout**
- Otimizador: **Adam**
- Função de perda: **CrossEntropyLoss**
- Early Stopping (patience = 10)

Três configurações foram testadas para comparação experimental.

## 3. Resultados

### 3.1 Resultados da Melhor Configuração (Baseline)

**Tabela 2: Relatório de Classificação - Baseline**

| Classe                | Precision | Recall   | F1-Score | Support |
|-----------------------|-----------|----------|----------|---------|
| No Depression (0)     | 0.9957    | 1.0000   | 0.9979   | 234     |
| Depression (1)        | 1.0000    | 0.8333   | 0.9091   | 6       |
| **Accuracy**          | -         | -        | **0.9958** | 240   |
| **Macro Avg**         | 0.9979    | 0.9167   | 0.9535   | 240     |
| **Weighted Avg**      | 0.9959    | 0.9958   | 0.9956   | 240     |

O modelo alcançou **99,58% de acurácia** e excelente F1-score.

(Inserir aqui as imagens: loss_curve_baseline.png e confusion_matrix_baseline.png)

## 4. Discussão

As variáveis relacionadas ao sono e ao uso de redes sociais se mostraram as mais preditivas, corroborando estudos da área de saúde mental. O alto desempenho do modelo indica que os dados são bastante separáveis. No entanto, o desbalanceamento da classe "Depression" (apenas 6 amostras no teste) sugere cautela na generalização.

O uso do pipeline MLOps com Weights & Biases permitiu rastreabilidade completa dos experimentos.

## 5. Conclusão

Este trabalho demonstrou a aplicação prática de um pipeline completo de Machine Learning com rede neural MLP e boas práticas de MLOps. As decisões técnicas foram justificadas experimentalmente e os resultados obtidos foram satisfatórios.

**Sugestões para trabalhos futuros:**
- Coleta de mais dados para a classe minoritária
- Experimentação com modelos baseados em árvores (XGBoost, LightGBM)
- Implementação de API para deployment do modelo

---

**Referências**  
(normas ABNT - adicionar se necessário)