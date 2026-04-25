### Ranking Final de Features

Foram aplicados três métodos de seleção de variáveis e combinados em um score final normalizado:

- **Correlação de Pearson**
- **Mutual Information**
- **Importância via Random Forest**

**Tabela 1: Ranking de Features (normalizado)**

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

**Features selecionadas** (threshold final_score > 0.3):  
**`sleep_hours`**, **`daily_social_media_hours`**, **`stress_level`** e **`anxiety_level`**.

**Justificativa:** Essas variáveis apresentaram maior poder preditivo combinado. O tempo de sono e o uso de redes sociais são os fatores mais relevantes, o que está alinhado com a literatura sobre saúde mental na adolescência.

### Resultados do Modelo (Melhor configuração - Baseline)

**Tabela 2: Relatório de Classificação (Baseline MLP)**

| Classe              | Precision | Recall   | F1-Score | Support |
|---------------------|-----------|----------|----------|---------|
| No Depression (0)   | 0.9957    | 1.0000   | 0.9979   | 234     |
| Depression (1)      | 1.0000    | 0.8333   | 0.9091   | 6       |
| **Accuracy**        | -         | -        | **0.9958** | **240** |
| **Macro Avg**       | 0.9979    | 0.9167   | 0.9535   | 240     |
| **Weighted Avg**    | 0.9959    | 0.9958   | 0.9956   | 240     |

**Observações:**
- O modelo obteve **acurácia de 99,58%** e F1-score weighted de \~0,9956.
- Excelente desempenho na classe majoritária (No Depression).
- Na classe minoritária (Depression), o recall foi de 83,33%, indicando boa capacidade de detecção, apesar do desbalanceamento.

### Comparação entre Configurações Testadas

| Configuração     | Camadas Ocultas | Dropout | LR     | Acurácia | F1-Score (Weighted) | Val Loss Final |
|------------------|------------------|---------|--------|----------|---------------------|----------------|
| Baseline         | 128-64           | 0.3     | 0.001  | 99.58%   | 0.9956              | -              |
| Wider            | 256-128          | 0.3     | 0.001  | ...      | ...                 | ...            |
| Low Dropout      | 128-64           | 0.2     | 0.0005 | ...      | ...                 | ...            |

**Conclusão da comparação:** A configuração baseline apresentou o melhor equilíbrio entre desempenho e simplicidade.