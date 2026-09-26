# Detecção de Fraude em Cartões de Crédito 💳

Projeto de Machine Learning para detecção de fraude em transações de cartão de crédito, desenvolvido como parte de um desafio da DIO.

## 📌 O problema

O dataset apresenta um desbalanceamento extremo: a classe de fraude representa uma pequena parcela das transações. Nesse cenário, a acurácia isoladamente pode criar uma impressão enganosa de desempenho.

Por isso, este projeto dá atenção especial às métricas da classe de fraude (`Class = 1`): **Precision, Recall e F1-score**.

- **Precision:** entre as transações marcadas como fraude, quantas realmente eram fraude.
- **Recall:** entre as fraudes reais, quantas foram identificadas pelo modelo.
- **F1-score:** média harmônica entre precision e recall.

## 🧹 Preparação dos dados

- Dataset carregado diretamente pela URL, sem armazenar `creditcard.csv` no repositório;
- Criação de `Log_Amount` com `np.log1p` para reduzir a assimetria do valor das transações;
- Divisão estratificada em treino, validação e teste;
- `StandardScaler` ajustado somente no conjunto de treino, evitando data leakage;
- Undersampling e SMOTE testados somente no conjunto de treino.

## 🤖 Modelos

Foram comparados:

1. Regressão Logística com `class_weight="balanced"`;
2. Regressão Logística com undersampling;
3. Regressão Logística com SMOTE;
4. Random Forest com `class_weight="balanced_subsample"`;
5. XGBoost com `scale_pos_weight`.

## 📊 Resultados no conjunto de teste

Os resultados abaixo foram gerados automaticamente pela execução deste notebook.

| Modelo | Precision (fraude) | Recall (fraude) | F1 (fraude) | ROC-AUC | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Regressão Logística (class_weight) | 0.0597 | 0.9184 | 0.1121 | 0.9725 | 0.7104 |
| Regressão Logística (undersampling) | 0.0348 | 0.9082 | 0.0670 | 0.9703 | 0.4417 |
| Regressão Logística (SMOTE) | 0.0563 | 0.8980 | 0.1059 | 0.9726 | 0.7204 |
| XGBoost (scale_pos_weight) | 0.5986 | 0.8673 | 0.7083 | 0.9770 | 0.8508 |
| Random Forest (class_weight) | 0.8352 | 0.7755 | 0.8042 | 0.9587 | 0.8294 |
| XGBoost — teste (threshold ajustado) | 0.8736 | 0.7755 | 0.8216 | 0.9770 | 0.8508 |

## 🎚️ Threshold do XGBoost

O threshold padrão de 0.5 foi comparado com um threshold ajustado. O valor ajustado foi escolhido **no conjunto de validação**, maximizando o F1 da classe de fraude.

**Threshold escolhido:** `0.989343`



No conjunto de teste, a comparação foi:

| Configuração | Precision | Recall | F1 |
|---|---:|---:|---:|
| Threshold 0.5 | 0.5986 | 0.8673 | 0.7083 |
| Threshold ajustado | 0.8736 | 0.7755 | 0.8216 |


## 🔍 SHAP

Na análise global, as cinco variáveis com maior impacto absoluto médio foram:

- `V4`
- `V14`
- `V12`
- `V10`
- `V11`


Para uma transação marcada como fraude pelo threshold ajustado, as maiores contribuições individuais foram:

- **V14**: Aumenta a saída do modelo para fraude (SHAP = 1.9198)
- **V10**: Aumenta a saída do modelo para fraude (SHAP = 1.5218)
- **V17**: Aumenta a saída do modelo para fraude (SHAP = 1.3650)
- **V16**: Aumenta a saída do modelo para fraude (SHAP = 0.9150)
- **V12**: Aumenta a saída do modelo para fraude (SHAP = 0.7758)


## 🚀 O que foi acrescentado/adaptado no projeto

- Separação explícita entre treino, validação e teste;
- Padronização sem usar informações do conjunto de teste;
- Comparação de undersampling e SMOTE;
- Inclusão de Random Forest e XGBoost;
- Ajuste de threshold baseado na validação, evitando usar o teste para escolher o limiar;
- Avaliação por Precision, Recall, F1, ROC-AUC e PR-AUC;
- SHAP global e explicação individual de uma transação;
- Salvamento das principais tabelas e figuras geradas pelo notebook.

## 📁 Estrutura do repositório

```text
.
├── deteccao_fraude_cartoes_colab.ipynb
├── README.md
├── requisitos.txt
├── resultados_modelos.csv
├── comparacao_threshold_xgboost.csv
└── figuras/
```

## ✅ Observações

- O dataset não é incluído no repositório.
- O notebook realiza o carregamento dos dados diretamente pela URL.
- Os arquivos `.csv` contêm os resultados gerados durante a execução do notebook.
