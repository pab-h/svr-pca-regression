# Previsão do Desempenho de Estudantes usando SVR e PCA

Este repositório contém a implementação completa de um experimento de **regressão com Support Vector Regression (SVR)**, avaliando o impacto da **Análise de Componentes Principais (PCA)** na previsão do desempenho acadêmico de estudantes.

O projeto foi desenvolvido como parte da disciplina **Tópicos Especiais em Telecomunicações I – Reconhecimento de Padrões**, do curso de **Engenharia de Computação**, seguindo rigorosamente as restrições propostas no enunciado do trabalho.


## 📌 Objetivo

Avaliar o desempenho de um regressor **SVR** na predição do **Índice de Desempenho** de estudantes, comparando:

* SVR sem redução de dimensionalidade
* SVR combinado com PCA utilizando **1 a 5 componentes principais**

A análise considera métricas quantitativas (**MSE** e **R²**) e qualitativas, discutindo o impacto da perda (ou preservação) de variância causada pelo PCA.


## 📊 Dataset

* **Fonte**: Student_Performance.csv (Kaggle)
* **Total de amostras**: 1000

### Variáveis de entrada

* **Hours Studied** – Horas de estudo
* **Previous Scores** – Notas anteriores
* **Extracurricular Activities** – Participação em atividades extracurriculares (Sim/Não)
* **Sleep Hours** – Horas médias de sono
* **Sample Question Papers Practiced** – Quantidade de provas simuladas praticadas

### Variável alvo

* **Performance Index** – Índice de desempenho acadêmico (10 a 100)

## ⚙️ Pré-processamento

* Codificação binária da variável categórica **Extracurricular Activities** usando `LabelBinarizer`
* Análise exploratória com:

  * Gráficos de barras para variáveis discretas
  * Gráfico de dispersão (*Previous Scores × Performance Index*)
  * Matriz de correlação


## 🔍 Seleção de Atributos – PCA

O **PCA foi implementado do zero**, sem uso de bibliotecas prontas, conforme exigido no trabalho.

Etapas:

1. Centralização dos dados
2. Cálculo da matriz de covariância
3. Decomposição em autovalores e autovetores
4. Ordenação por variância explicada
5. Projeção nos componentes principais

Foram testados:

* **1, 2, 3, 4 e 5 componentes principais**

Também foi calculada a **razão de variância explicada** para cada configuração.

## 🧠 Modelo de Regressão – SVR

### Otimização de hiperparâmetros

Ajuste realizado **exclusivamente no SVR sem PCA**, utilizando:

* **Grid Search manual** (sem funções prontas)
* **Validação cruzada K-Fold (k = 10)**

#### Parâmetros testados

* **Kernel**: linear, rbf, polinomial (grau 2)
* **C**: 10⁻², 10⁻¹, 1, 10, 100
* **Gamma**: 10⁻², 10⁻¹, 1, 10, 100

O melhor conjunto de hiperparâmetros foi reutilizado nos modelos com PCA, conforme especificado no enunciado.

## 📐 Métricas de Avaliação

Para cada um dos **6 regressores** (SVR puro + 5 com PCA), foram calculados:

* **Erro Quadrático Médio (MSE)**
* **Coeficiente de determinação (R²)**
* **Razão de variância explicada pelo PCA**

## 📈 Resultados Principais

### SVR sem PCA (modelo base)

* **R² ≈ 0.989**
* **MSE ≈ 4.07**

### SVR + PCA

| Componentes |  | MSE | Observação |
| --- | --- | --- | --- |
| 1 | ~0.84 | ~58.7 | Alta perda de informação |
| 2 | ~0.85 | ~56.7 | Pequena melhora |
| **3** | **~0.99** | **~4.8** | **Desempenho próximo ao original** |
| 4 | ~0.99 | ~4.2 | Ganhos marginais |
| 5 | ~0.99 | ~4.07 | Variância total preservada |


## 📝 Discussão

* O PCA com poucos componentes captura dimensões altamente relevantes (principalmente **Previous Scores**).
* A partir de **3 componentes**, o SVR praticamente recupera o desempenho do modelo original.
* Com **5 componentes**, o PCA não reduz dimensionalidade, mas **remove correlações**, o que pode beneficiar a regressão.
* O experimento demonstra claramente o **trade-off entre redução dimensional e desempenho preditivo**.
