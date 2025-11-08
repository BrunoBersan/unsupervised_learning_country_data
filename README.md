# Clusterização de Países: Análise Socioeconômica

Este projeto utiliza diversas técnicas de **Aprendizado Não Supervisionado** (Clustering) para agrupar países com base em indicadores de saúde, economia e desenvolvimento. O objetivo principal é identificar padrões de similaridade entre as nações, classificando-as em grupos como "Desenvolvidos", "Em Desenvolvimento" e "Subdesenvolvidos".

## Tecnologias e Bibliotecas

| Categoria | Pacotes |
| :--- | :--- |
| **Linguagem** | Python (versão 3.11.14) |
| **Gerenciamento** | `requirements.txt` (exportado via `!pip freeze`) |
| **Aquisição de Dados** | `kaggle` |
| **Manipulação/Análise** | `pandas`, `numpy` |
| **Visualização** | `matplotlib.pyplot`, `seaborn` |
| **Pré-processamento** | `sklearn.preprocessing.StandardScaler` |
| **Algoritmos de Clustering** | `sklearn.cluster.KMeans`, `scipy.cluster.hierarchy` (Hierárquico), `sklearn.cluster.DBSCAN` |

---

## Conjunto de Dados

O dataset utilizado, **"Unsupervised Learning on Country Data"** (`Country-data.csv`), foi obtido diretamente da API do Kaggle.

* **Fonte:** `rohan0301/unsupervised-learning-on-country-data`
* **Estrutura:** 167 entradas e 10 colunas. Não foram encontrados valores ausentes ou duplicados na análise inicial.

### Descrição das Variáveis

A análise é baseada nas seguintes *features* socioeconômicas:

| Variável | Descrição |
| :--- | :--- |
| **country** | Nome do país. |
| **child_mort** | Mortes de crianças menores de 5 anos por 1.000 nascidos vivos. |
| **exports** | Exportações de bens e serviços (% do PIB per capita). |
| **health** | Gastos totais em saúde (% do PIB per capita). |
| **imports** | Importações de bens e serviços (% do PIB per capita). |
| **income** | Renda líquida per capita (USD). |
| **inflation** | Taxa anual de crescimento do PIB (%). |
| **life_expec** | Expectativa de vida ao nascer (anos). |
| **total_fer** | Número médio de filhos por mulher. |
| **gdpp** | PIB per capita (USD). |

---

## Metodologia e Processamento

### 1. Pré-processamento e Escalonamento

A etapa de **clusterização** é sensível à escala dos dados. A análise de boxplot inicial demonstrou uma grande **faixa dinâmica de valores** e a presença de **outliers**, o que poderia distorcer as métricas de distância (e.g., K-Means).

* **Ação:** A coluna `country` foi removida, e os dados numéricos (`df_num`) foram **padronizados** (Standardization) utilizando o **`StandardScaler`** da Scikit-learn.
* **Impacto:** O escalonamento garante que todas as variáveis tenham uma **influência equivalente** na determinação dos clusters.

---

### 2. Algoritmos de Clusterização

Foram aplicados quatro métodos para garantir a robustez dos resultados e fornecer diferentes perspectivas sobre a estrutura dos dados:

#### A. K-Means (K=3)

| Métrica | Cluster 0 (Desenvolvidos) | Cluster 1 (Subdesenv.) | Cluster 2 (Intermediários) |
| :--- | :--- | :--- | :--- |
| **Países** | 36 | 47 | 84 |
| **child_mort** | 5.00 | **92.96** | 21.93 |
| **income** | **45672.22** | 3942.40 | 12305.60 |
| **life_expec** | **80.13** | 59.19 | 72.81 |
| **total_fer** | 1.75 | **5.01** | 2.31 |
| **gdpp** | **42494.44** | 1922.38 | 6486.45 |

#### B. Hierárquico (Ward Linkage)

O dendrograma foi utilizado para visualizar a similaridade hierárquica e sugerir um corte em **3 grupos principais**.

#### C. K-Medoids (Simulação de PAM, K=3)

Este método é uma variação do K-Means mais robusta a *outliers*, utilizando **medoides** (pontos reais do dataset) em vez de centróides (pontos médios).

* **Países Representativos (Medoides Finais):**
    * **Cluster 0:** Kiribati
    * **Cluster 1:** Ghana
    * **Cluster 2:** Poland

#### D. DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

O DBSCAN agrupa pontos baseados em densidade, identificando automaticamente clusters de formato arbitrário e, crucialmente, marcando **outliers** como ruído (Cluster -1).

* **Parâmetros:** `eps=0.5`, `min_samples=5`.
* **Resultados:** Identificou **3 grupos densos** (Clusters 0, 1 e 2) e um grupo de **outliers** (-1).

---

## Perfis dos Clusters (DBSCAN - Sem Outliers)

Os resultados do DBSCAN confirmam os **três grupos socioeconômicos principais** e fornecem perfis claros (as métricas a seguir são médias por cluster, com `income` e `gdpp` em USD):

### 1. Cluster 0 – Países Subdesenvolvidos (Pobreza Extrema) 

* **País mais representativo:** 🇲🇿 Mozambique
* **Características:** Alta **mortalidade infantil (94,19)** e **fertilidade (5,34)**. Baixa **renda (2.039,22 USD)**, **PIB per capita (811,74 USD)** e **expectativa de vida (59,40 anos)**.
* **Análise:** Grupo com os piores indicadores, representando países com **pobreza severa**, alta dependência de **importações** e grandes desafios de desenvolvimento humano.

### 2. Cluster 1 – Países em Desenvolvimento (Situação Intermediária) 

* **País mais representativo:** 🇸🇻 El Salvador
* **Características:** Mortalidade infantil e fertilidade **moderadas**. Renda e PIB per capita **intermediárias**.
* **Análise:** Países com indicadores de desenvolvimento **melhores que o Cluster 0**, mas **significativamente abaixo do Cluster 2**. Demonstram investimentos em **educação, saúde e exportações**, mas ainda enfrentando desafios de **renda e produtividade**.

### 3. Cluster 2 – Países Desenvolvidos 

* **País mais representativo:** 🇬🇧 United Kingdom
* **Características:** Baixa **mortalidade infantil (4,17)** e **fertilidade (1,80)**. Alta **renda (37.338,89 USD)**, **PIB per capita (41.150,00 USD)** e **expectativa de vida (81,01 anos)**.
* **Análise:** Este grupo representa países **desenvolvidos**, com **alta renda**, forte **investimento social** e **estabilidade econômica**. O **Reino Unido** reflete bem o centro do grupo.

---

## Comparação de Métodos

Todos os métodos identificaram **três grupos socioeconômicos principais**, com perfis muito semelhantes.

* **Tamanho dos Grupos:**
    * **K-Means:** Grupos mais equilibrados (36, 47 e 84 países).
    * **Hierárquico (Ward):** Grupos com maior variância (106, 34 e 27 países), mostrando uma fusão mais ampla de países intermediários.
    * **DBSCAN:** Identificou 3 clusters (96, 62 e 9 países) + outliers.
* **Consistência:** A correspondência entre os clusters é **alta** (validação cruzada implícita, como a média de `Cluster_KMeans` dentro dos grupos Hierárquicos sendo **0.18, 1.00 e 1.72**).
* **Vantagens:** O método hierárquico oferece uma **visão mais interpretável e visual (dendrograma)**, enquanto o K-Means é **mais direto e eficiente computacionalmente**. O DBSCAN, por sua vez, é excelente para identificar **outliers**.
