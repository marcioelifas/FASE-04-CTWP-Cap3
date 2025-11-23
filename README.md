# 🌾 Classificação de Grãos com Machine Learning (CRISP-DM)

**FIAP - Fase 4 - Capítulo 3: Da Terra ao Código**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1q_dA4_s58MoirCRGXitxJrriDpr2US8e?usp=sharing)

## 👥 Integrantes do Grupo

* **Giovani Saavedra** - RM566797
* **Marcio Elifas** - RM567871
* **Felipe Bernardo Papeleo de Oliveira** - RM567782

---

## 1. 🎯 Objetivo do Projeto

O objetivo desta atividade é aplicar a metodologia **CRISP-DM** (Cross Industry Standard Process for Data Mining) para desenvolver um modelo de aprendizado de máquina capaz de classificar automaticamente variedades de grãos de trigo.

Em cooperativas agrícolas, essa classificação é frequentemente manual, o que é sujeito a erros e lentidão. Automatizar esse processo aumenta a eficiência e a precisão da triagem.

Utilizamos o **"Seeds Dataset"** (do UCI Machine Learning Repository), que contém medições geométricas de grãos pertencentes a três variedades:
1. **Kama**
2. **Rosa**
3. **Canadian**

---

## 2. 🔍 Entendimento e Análise dos Dados (Data Understanding)

A primeira etapa do projeto consistiu na importação e análise exploratória dos dados. O dataset original (`seeds_dataset.txt`) não possui cabeçalho, portanto, definimos manualmente as colunas baseadas na documentação oficial:
* Área, Perímetro, Compacidade, Comprimento do Núcleo, Largura do Núcleo, Coeficiente de Assimetria e Comprimento do Sulco.

### Principais Análises Realizadas:
* **Estatísticas Descritivas:** Cálculo de média, desvio padrão e quartis para entender a dispersão dos dados.
* **Matriz de Correlação:** Identificamos que variáveis como *Área*, *Perímetro* e *Comprimento do Núcleo* possuem correlação positiva muito forte (próxima de 1.0), o que é esperado fisicamente.
* **Visualização Gráfica:** Utilizamos *Pairplots* e *Heatmaps* para observar que as classes possuem separações visuais claras em algumas dimensões, facilitando o trabalho dos algoritmos de classificação.

---

## 3. ⚙️ Pré-processamento (Data Preparation)

Para garantir que os modelos performem corretamente, aplicamos as seguintes transformações nos dados:

1. **Separação de Dados:** Dividimos o dataset em conjuntos de **Treino (70%)** e **Teste (30%)**. Utilizamos estratificação (`stratify`) para garantir que a proporção das três classes de trigo fosse mantida idêntica em ambos os conjuntos.
2. **Padronização (Feature Scaling):** Aplicamos o `StandardScaler`.
    * *Por que?* Algoritmos baseados em distância (como KNN e SVM) são sensíveis à escala. A variável "Área" (ex: 15.0) é numericamente muito maior que a "Assimetria" (ex: 0.8). Sem padronização, o modelo daria peso excessivo à Área. A padronização coloca todas as variáveis na mesma escala matemática.

---

## 4. 🤖 Modelagem (Modeling)

Implementamos e comparamos três algoritmos de classificação distintos para validar qual abordagem se adapta melhor ao problema:

### 1. K-Nearest Neighbors (KNN)
* **Lógica:** Classifica o grão baseando-se na classe dos vizinhos mais próximos no espaço geométrico.
* **Resultado:** Demonstrou alta eficácia, pois grãos da mesma espécie tendem a ter medidas agrupadas.

### 2. Support Vector Machine (SVM)
* **Lógica:** Busca traçar hiperplanos que separam as classes com a maior margem possível.
* **Configuração Inicial:** Kernel Linear.

### 3. Random Forest
* **Lógica:** Cria uma "floresta" de árvores de decisão e define a classe por votação majoritária.
* **Vantagem:** É robusto e fornece insights sobre a importância de cada característica (feature importance).

---

## 5. 🚀 Otimização e "Ir Além"

Para superar a performance dos modelos base, aplicamos a técnica de **Otimização de Hiperparâmetros** utilizando o `GridSearchCV` no modelo SVM.

* **Parâmetros testados:**
    * `C`: [0.1, 1, 10, 100] (Penalidade de erro)
    * `gamma`: [1, 0.1, 0.01, 0.001] (Coeficiente do kernel)
    * `kernel`: ['rbf', 'linear'] (Tipo de separação)

O GridSearch testou todas as combinações matematicamente possíveis para encontrar a configuração ótima para este dataset específico.

---

## 6. 📊 Conclusão e Insights

Após a execução dos testes e avaliação das matrizes de confusão, concluímos que:

1. **Alta Acurácia:** É possível classificar variedades de trigo com precisão superior a 90% utilizando apenas suas medidas geométricas.
2. **Distinção das Classes:** A variedade **Rosa** é a mais fácil de identificar, geralmente apresentando área e perímetro maiores. As variedades **Kama** e **Canadian** possuem algumas sobreposições sutis, onde ocorreram os poucos erros de classificação.
3. **Importância do Pré-processamento:** A normalização dos dados foi um passo crítico para o sucesso dos modelos KNN e SVM.

---

## 💻 Como Executar o Projeto

Você pode visualizar e executar o código diretamente no navegador através do Google Colab, ou rodar localmente.

### Opção 1: Google Colab
Clique no badge abaixo:
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1q_dA4_s58MoirCRGXitxJrriDpr2US8e?usp=sharing)

### Opção 2: Localmente
1. Clone o repositório.
2. Instale as dependências:
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn gdown

