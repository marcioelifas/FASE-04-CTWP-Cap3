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

## 6. 📈 Interpretação dos Resultados e Insights de Negócio

Atendendo ao objetivo de automatizar a triagem manual em cooperativas, realizamos uma análise aprofundada dos resultados obtidos pelos modelos:

### 1. Desempenho Comparativo
* **SVM e Random Forest:** Ambos apresentaram desempenho superior (geralmente acima de 92% de acurácia). O **SVM com Kernel Linear** se destacou, indicando que as características físicas dos grãos possuem separação linear clara em um espaço multidimensional.
* **KNN:** Embora eficaz, teve desempenho ligeiramente inferior, provavelmente devido à sensibilidade a *outliers* ou fronteiras de decisão menos definidas entre grãos de tamanhos intermediários.

### 2. Análise da Matriz de Confusão (Onde o modelo erra?)
Ao analisar os erros, percebemos um padrão comportamental consistente com a realidade física:
* **Variedade Rosa:** O modelo acerta quase 100% dos casos. **Motivo:** Esta é a variedade fisicamente maior (maior área e perímetro). É fácil para a máquina (e para humanos) distingui-la das demais.
* **Confusão Kama vs. Canadian:** A maioria dos erros do modelo ocorre entre estas duas variedades. **Insight:** Isso revela que estas espécies possuem características geométricas muito similares. No processo manual, é provável que especialistas humanos também cometam erros justamente nessas duas classes, o que justifica o uso da IA para reduzir a fadiga e padronizar a decisão.

### 3. Impacto no Negócio (A Solução para a Cooperativa)
Conectando os dados ao problema real da cooperativa agrícola:
* **Eficiência Operacional:** O modelo é capaz de classificar centenas de grãos em milissegundos, tarefa que levaria minutos ou horas para um humano.
* **Redução de Custo:** A automação permite que os especialistas foquem em tarefas de maior valor agregado, deixando a triagem repetitiva para o algoritmo.
* **Confiabilidade:** A consistência do modelo (acurácia > 90%) garante que a padronização dos lotes de trigo seja mantida, valorizando o produto final da cooperativa no mercado.

### Veredito Final
A implementação do modelo **SVM Otimizado** é a recomendação final para a solução tecnológica, pois equilibra alta precisão com baixo custo computacional, resolvendo o gargalo de classificação manual da cooperativa.
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

