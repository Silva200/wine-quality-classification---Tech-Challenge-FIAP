# 🍷 Wine Quality Classification — Tech Challenge (Fase 2 | POSTECH)

Pipeline de análise exploratória e classificação binária da qualidade de vinhos,
desenvolvido para o Tech Challenge da disciplina de Data Analytics.

## Descrição do Projeto

A indústria vitivinícola avalia a qualidade dos vinhos tradicionalmente por meio
de análise sensorial de especialistas — um processo subjetivo e demorado. Este
projeto usa dados físico-químicos do **Wine Quality Dataset** (UCI / Kaggle) para
treinar modelos de Machine Learning capazes de prever se um vinho é de **Alta
Qualidade** (nota ≥ 7) ou de **Baixa/Média Qualidade** (nota < 7), apoiando
enólogos e produtores na tomada de decisão.

## Objetivo

Desenvolver e comparar modelos de classificação binária capazes de prever a
qualidade de um vinho a partir de suas características físico-químicas
(acidez, teor alcoólico, densidade, dióxido de enxofre, etc.).

## Estrutura do Repositório

```
wine-quality-classification/
│
├── data/                # Base de dados utilizada (Wine Quality Dataset)
├── notebooks/           # Notebook com a análise exploratória e modelagem
│   └── wine_quality_binary_classification.ipynb
├── src/                 # Scripts auxiliares (reservado para uso futuro)
├── results/             # Gráficos e métricas exportadas dos modelos
├── requirements.txt     # Bibliotecas utilizadas
└── README.md            # Este arquivo
```

## Dataset

- **Fonte:** [Wine Quality Dataset](https://archive.ics.uci.edu/dataset/186/wine+quality) (UCI Machine Learning Repository / Kaggle), variante *red wine*.
- **Amostras:** 1.599 vinhos tintos originalmente (1.359 após remoção de 240
  linhas duplicadas identificadas no pré-processamento).
- **Variáveis:** 11 características físico-químicas (acidez fixa, acidez volátil,
  ácido cítrico, açúcar residual, cloretos, dióxido de enxofre livre e total,
  densidade, pH, sulfatos, teor alcoólico) + variável alvo `quality` (nota de
  0 a 10, atribuída por especialistas).
- **Transformação do alvo:** a nota `quality` é convertida em `quality_binary`
  (1 = Alta Qualidade, nota ≥ 7 | 0 = Baixa/Média Qualidade, nota < 7).
- **Observação:** o notebook carrega o dataset diretamente de uma URL pública
  (mirror do dataset no GitHub), portanto não é necessário baixar nenhum
  arquivo manualmente antes de executar.

## Pipeline Desenvolvido

1. **Compreensão do problema** — contexto de negócio, definição da variável alvo e
   transformação em classificação binária.
2. **Análise Exploratória de Dados (EDA)** — distribuição das variáveis,
   detecção de outliers (IQR), matriz de correlação (Spearman) e análise de
   balanceamento de classes.
3. **Pré-processamento** — checagem de valores faltantes, remoção de **240
   linhas duplicadas** (15% do dataset), criação de novas features
   (`acidity_total`, `free_sulfur_ratio`, `alcohol_density_ratio`), split
   estratificado treino/teste e padronização (`StandardScaler`).
4. **Modelagem** — treinamento e validação cruzada (5 folds) de 3 modelos:
   Regressão Logística, Random Forest e Gradient Boosting (todos com
   `class_weight='balanced'` para lidar com o desbalanceamento das classes).
5. **Avaliação** — comparação por acurácia, precisão, recall, F1-score,
   AUC-ROC, matriz de confusão e curva ROC.
6. **Interpretação** — importância de variáveis extraída dos modelos e
   discussão das implicações para o processo produtivo.

## Principais Resultados

| Modelo | Acurácia | Precisão | Recall | F1-Score | AUC-ROC |
|---|---|---|---|---|---|
| Regressão Logística | 0.772 | 0.344 | 0.764 | 0.475 | 0.860 |
| Gradient Boosting | 0.882 | 0.606 | 0.364 | 0.455 | **0.860** |
| Random Forest | 0.877 | 0.593 | 0.291 | 0.390 | 0.861 |

- Após a remoção das linhas duplicadas, os três modelos ficaram muito próximos
  em AUC-ROC (~0.86). A **Regressão Logística** obteve o melhor F1-Score e o
  maior recall — captura mais vinhos de alta qualidade, ao custo de mais falsos
  positivos —, enquanto **Gradient Boosting** e **Random Forest** têm maior
  precisão (menos falsos positivos, mas identificam menos vinhos bons).
- A escolha do modelo final depende do custo de negócio de cada tipo de erro
  (ver discussão no notebook, Seção 8).
- As variáveis com maior influência na qualidade do vinho, consistentes entre
  os 3 modelos, foram: **`alcohol`**, **`sulphates`** e **`volatile acidity`**.

## Como Executar

```bash
# 1. Clonar o repositório
git clone <url-do-repositorio>
cd wine-quality-classification

# 2. Criar ambiente virtual (opcional, recomendado)
python -m venv venv
source venv/bin/activate    # Linux/Mac
venv\Scripts\activate       # Windows

# 3. Instalar as dependências
pip install -r requirements.txt

# 4. Abrir o notebook
jupyter notebook notebooks/wine_quality_binary_classification.ipynb
```

O notebook baixa o dataset automaticamente via URL pública ao ser executado —
não é necessário nenhum arquivo local em `data/` para rodá-lo, mas o
diretório é mantido na estrutura para armazenar uma cópia local, se desejado.

## Entregáveis do Tech Challenge

- ✅ Notebook com a análise e modelagem (`notebooks/`)
- ⬜ Apresentação executiva (PPT/PDF) com o storytelling da EDA — a ser
  adicionada em `results/`
- ⬜ Vídeo executivo de até 5 minutos — link a ser adicionado neste README

## Autores

Felipe Paulussi da Silva — Fase 2 | POSTECH

