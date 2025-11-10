# Projeto de Clusterização de Países com Algoritmos de Aprendizado Não Supervisionado

## Autor

**Bruno Adalberto dos Santos**

---

## 1. Descrição do Projeto

Este projeto tem como objetivo aplicar técnicas de **aprendizado não supervisionado** para agrupar países com base em indicadores socioeconômicos e de saúde, identificando padrões de desenvolvimento e vulnerabilidade global.
A análise foi conduzida em Python, utilizando os algoritmos **K-Means**, **Clusterização Hierárquica**, **K-Medoids** e **DBSCAN**, com o objetivo de comparar os resultados e interpretar a consistência entre os métodos.

Os dados foram obtidos do Kaggle, a partir do conjunto:
[Unsupervised Learning on Country Data](https://www.kaggle.com/datasets/rohan0301/unsupervised-learning-on-country-data)

---

## 2. Infraestrutura do Projeto

**Ambiente de Desenvolvimento:**

* Linguagem: Python 3.9+
* Ambiente Virtual: Anaconda (ou Virtualenv)
* IDE: Visual Studio Code
* Repositório: [https://github.com/BrunoBersan/unsupervised_learning_country_data](https://github.com/BrunoBersan/unsupervised_learning_country_data)

**Arquivo de Dependências:**
O arquivo `requirements.txt` foi gerado automaticamente com:

```bash
!pip freeze > requirements.txt
```

---

## 3. Estrutura do Dataset

O dataset contém **167 países**, descritos pelas seguintes variáveis:

| Variável     | Descrição                                       |
| ------------ | ----------------------------------------------- |
| `country`    | Nome do país                                    |
| `child_mort` | Mortalidade infantil (por 1.000 nascidos vivos) |
| `exports`    | Exportações (% do PIB per capita)               |
| `health`     | Gastos totais em saúde (% do PIB per capita)    |
| `imports`    | Importações (% do PIB per capita)               |
| `income`     | Renda líquida per capita (USD)                  |
| `inflation`  | Taxa anual de crescimento do PIB (%)            |
| `life_expec` | Expectativa de vida ao nascer (anos)            |
| `total_fer`  | Número médio de filhos por mulher               |
| `gdpp`       | PIB per capita (USD)                            |

---

## 4. Pré-Processamento dos Dados

Antes da aplicação dos algoritmos de clusterização, foram realizadas as seguintes etapas:

1. **Limpeza dos Dados:** remoção de valores ausentes e duplicados.
2. **Seleção de Variáveis:** apenas variáveis numéricas foram mantidas.
3. **Normalização:** escalonamento dos dados com `StandardScaler`.
4. **Tratamento de Outliers:** análise via boxplots e estatística descritiva.
5. **Análise da Distribuição:** avaliação da adequação dos dados para métodos baseados em distância.

---

## 5. Algoritmos de Clusterização

### 5.1. K-Means

Método particional que busca minimizar a distância média dos pontos em relação aos centróides.
Foi configurado para **3 clusters** e converge quando o deslocamento médio dos centróides é mínimo.

### 5.2. Clusterização Hierárquica

Método baseado em fusões sucessivas (linkage **Ward**), gerando um dendrograma que mostra as relações de similaridade entre os países.

### 5.3. K-Medoids

Variação do K-Means em que o centro de cada grupo é um **ponto real do conjunto de dados (medoide)**, o que melhora a interpretabilidade e reduz a influência de outliers.

### 5.4. DBSCAN (Density-Based Spatial Clustering of Applications with Noise)

O DBSCAN forma clusters com base na **densidade local dos pontos**, agrupando regiões densas e marcando como ruído os pontos isolados.
Parâmetros principais:

* `eps`: raio máximo de vizinhança.
* `min_samples`: número mínimo de pontos em uma vizinhança para formar um cluster.

**Vantagens do DBSCAN:**

* Não requer definir o número de clusters a priori.
* Identifica automaticamente **outliers (ruído)**.
* Funciona bem com clusters de formas arbitrárias.
* É **mais robusto a outliers** que K-Means, pois não depende da média.

---

## 6. Resultados da Clusterização

### 6.1. K-Means

| Cluster | Descrição                                                                                       | País Representativo |
| ------- | ----------------------------------------------------------------------------------------------- | ------------------- |
| **0**   | Países desenvolvidos, com alta renda, baixa mortalidade infantil e elevada expectativa de vida. | Iceland             |
| **1**   | Países em vulnerabilidade socioeconômica, com alta mortalidade infantil e baixa renda.          | Guinea              |
| **2**   | Economias emergentes, com rendas intermediárias e transição social.                             | Jamaica             |

---

### 6.2. Clusterização Hierárquica

| Cluster | Descrição            | País Representativo |
| ------- | -------------------- | ------------------- |
| **0**   | Países desenvolvidos | Poland              |
| **1**   | Países vulneráveis   | Ghana               |
| **2**   | Economias emergentes | Kiribati            |

---

### 6.3. K-Medoids

| Cluster | Descrição                                | País Representativo |
| ------- | ---------------------------------------- | ------------------- |
| **0**   | Países em vulnerabilidade socioeconômica | Mozambique          |
| **1**   | Economias emergentes                     | El Salvador         |
| **2**   | Países desenvolvidos                     | United Kingdom      |

---

### 6.4. DBSCAN

O DBSCAN identificou **três grandes grupos principais** e **um conjunto de outliers** (países com características muito distintas, como renda extremamente alta ou baixa).
Por não exigir número fixo de clusters, o algoritmo capturou nuances adicionais nas regiões de transição entre economias.

**Principais observações:**

* Países com perfis muito distintos (ex: Luxemburgo, Somália) foram classificados como ruído, o que melhora a consistência dos agrupamentos restantes.
* O DBSCAN apresentou **grande robustez a outliers** e produziu clusters coerentes com os de K-Means, porém com fronteiras mais flexíveis.
* A distribuição espacial dos clusters mostrou padrões semelhantes aos demais métodos, reforçando a validade da estrutura global dos dados.

---

## 7. Interpretação dos Clusters

### Cluster 0 – Países Vulneráveis

Alta mortalidade infantil, baixa renda e expectativa de vida, elevada fertilidade e inflação.
**Exemplo:** *Mozambique*

### Cluster 1 – Economias Emergentes

Renda e expectativa de vida intermediárias, mortalidade infantil moderada, inflação controlada.
**Exemplo:** *El Salvador*

### Cluster 2 – Países Desenvolvidos

Alta renda, baixa mortalidade e fertilidade, elevada expectativa de vida e alto investimento em saúde.
**Exemplo:** *United Kingdom*

---

## 8. Comparação entre os Métodos

| Critério                        | K-Means  | Hierárquico             | K-Medoids           | DBSCAN      |
| ------------------------------- | -------- | ----------------------- | ------------------- | ----------- |
| **Equilíbrio dos grupos**       | Moderado | Menos equilibrado       | Desequilibrado      | Adaptativo  |
| **Forma dos clusters**          | Esférica | Hierárquica             | Baseada em medoides | Arbitrária  |
| **Número de clusters definido** | Sim      | Sim                     | Sim                 | Não         |
| **Sensibilidade a outliers**    | Alta     | Média                   | Baixa               | Muito baixa |
| **Interpretação visual**        | Boa      | Excelente (dendrograma) | Alta                | Média       |
| **Robustez estrutural**         | Alta     | Alta                    | Alta                | Muito alta  |

---

## 9. Considerações Finais

* Todos os métodos identificaram **padrões socioeconômicos coerentes**.
* O **K-Means** mostrou eficiência, mas sensibilidade a outliers.
* O **Hierárquico** se destacou na visualização das relações entre países.
* O **K-Medoids** trouxe clareza interpretativa, por usar países reais como centros.
* O **DBSCAN** apresentou **maior robustez a ruído** e revelou **estruturas complexas** que os outros métodos não capturaram.

A análise reforça que a clusterização não supervisionada é uma ferramenta poderosa para **entendimento de padrões globais de desenvolvimento**, permitindo insights claros sobre desigualdade e crescimento entre nações.

---

## 10. Repositório e Reprodutibilidade

Repositório público com o código, notebook e artefatos:
[https://github.com/BrunoBersan/unsupervised_learning_country_data](https://github.com/BrunoBersan/unsupervised_learning_country_data)

Para executar o projeto:

```bash
git clone https://github.com/BrunoBersan/unsupervised_learning_country_data
cd unsupervised_learning_country_data
pip install -r requirements.txt
jupyter notebook analise.ipynb
``` 