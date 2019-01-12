---
layout: post
title: Detecção de Fraude
date: 2019-01-03 13:00:00
author: Talita Shiguemoto
description: Projeto que detecta transações fraudulentas de cartão de crédito 
comments: true
ref: fraudDetection
lang: pt
---

Muitas transações por cartão de crédito são fraudulentas, o objetivo deste projeto é detectá-las baseado em um conjunto de dados histórico que já possui tal classificação (se foi fraude ou não). Cada transação possui algumas variáveis e com base nelas poderemos identificar quais atributos possuem maior influência para desvendar quais são fraudes ou não. Devido esse histórico com características o modelo criado será baseado em uma técnica de Machine Learning chamada aprendizagem supervisionada e poderá ser utilizado para futuras detecções de fraudes.

O primeiro passo será pré-processar os dados para lidarmos com o desbalanceamento que, em outras palavras, é o fato de que normalmente em transações por cartão de crédito a taxa de fraudes é muito menor que a taxa de transações verdadeiras. Portanto, iremos utilizar uma técnica de subamostragem para resolver este impasse. Feito isso, será testado os algoritmos de Métodos de Ensemble (Random Forest, Gradient Boosting, XGBoost),Support Vector Machines e K-Nearest Neighbors, verificando qual deles terá melhor performance e resultados. O modelo final será escolhido embasado por algumas métricas (veja em **Métricas de avaliação**) e terá seus hiperparâmetros otimizados pelo *GridSearch*.

### **Métricas de avaliação**
<br/>
Devido nosso conjunto ser desbalanceado a acurácia não é a melhor métrica para ser usada na avaliação do modelo. Para este estudo utilizaremos principalmente o AUC ROC para verificar a qualidade do algoritmo.

O *recall*, também chamado de ***True Positive Rate (TPR)***, é a proporção entre os Verdadeiros Positivos sobre todas os positivos classificados corretamente e os falsos negativos. Matematicamente representado por:

\begin{align}
\dot{Recall} & = 	\frac{TP}{TP+FN} 
\end{align}

Para entender o AUC é necessário compreender primeiro a curva do ***Receiver Operating Characteristic (ROC)***, que é um gráfico que mostra o desempenho de um modelo de classificação em todos os limites de classificação. Esta curva mostra dois parâmetros: o TPR e o False Positive Rate (FPR).

\begin{align}
\dot{FRP} & = 	\frac{FP}{TP+TN} 
\end{align}

Uma curva ROC traça TPR vs. FPR em diferentes limiares de classificação. Diminuir o limiar de classificação classifica mais itens como positivos, aumentando assim tanto os falsos positivos quanto os verdadeiros positivos. Em outras palavras, um modelo prediz a probabilidade de uma classe ser 1 ou 0, usando essas probabilidades é possível plotar um gráfico de distribuição como na figura abaixo, sendo a curva vermelha representando 0 e a verde para 1 , sendo 0,5 o limite entre as duas classes.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_01.png" alt="" title="Figura 1 - ROC"/>
</div>
<div class="col three caption">
	Figura 1 - ROC
</div>
<br/>

Todos os valores positivos acima do limite (maiores que 0.5) serão True Positives (TP), e todos os valores negativos acima do limite serão False Positives (FP), uma vez que foram classificados incorretamente como positivos. Abaixo do limite, todos os valores negativos serão True Negatives (TN) e os positivos False Negatives (FN), uma vez que foram classificados incorretamente como negativos. Esse conceito é melhor demonstrado na figura a seguir.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_02.png" alt="" align="center" title="Figura 2 - TN, TP, FN, FP"/>
</div>
<div class="col three caption">
	Figura 2 - TN, TP, FN, FP
</div>
<br/>

A curva AUC mede toda a área bidimensional por baixo de toda curva ROC. AUC fornece uma medida agregada de desempenho em todos os possíveis limites de classificação. Uma maneira de interpretar AUC é como a probabilidade de o modelo classificar um exemplo positivo aleatório mais do que um exemplo negativo aleatório. Um modelo cujas previsões são 100% erradas tem uma AUC de 0,0; enquanto um cujas previsões são 100% corretas tem uma AUC de 1,0. Conforme Figura 3.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_03.png" alt="" title="Figura 3 - ROC AUC"/>
</div>
<div class="col three caption">
	Figura 3 - ROC AUC
</div>
<br/>

Outra métrica utilizada para escolha do melhor modelo será a utilização do *Classification Report* do sklearn, trata-se de uma tabela de reporte com as principais métricas de avaliação para cada classe, sendo elas: recall, precision e *f1_score*.
O *precision* é a proporção entre os Verdadeiros Positivos sobre todas os positivos, sendo eles classificados corretamente ou não. Matematicamente representado por:

\begin{align}
\dot{Precision} & = 	\frac{TP}{TP+FP} 
\end{align}

O f1_score é uma medida que considera ambos: precision e recall, sendo sua fórmula expressada por:

\begin{align}
\dot{f1 score} & = 2 \cdot	\frac{precision \cdot recall}{precision + recall} 
\end{align}

### **Exploração dos Dados**
<br/>
O conjunto de dados é referente a transações de usuários de cartões europeus em setembro de 2013, obtidos no Kaggle. Possuindo 284.807 linhas e 31 features, das quais 28 delas são variáveis dependentes que devido a confidencialidade são resultados de transformações de PCA. Podemos verificar em detalhes cada feature na Tabela 1.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_12_pt.jpg" alt="" title="Tabela 1 - Descrição do Conjunto de Dados"/>
</div>
<div class="col three caption">
	Tabela 1 - Descrição do Conjunto de Dados
</div>
<br/>

Não há nenhum *missing values* no conjunto de dados, contudo os dados estão desbalanceados como podemos observar na Figura 4, sendo que a variável *target Class* possui valores binários, 0 para transações normais e 1 para fraudulentas, na qual a segunda representa 0,17% dos dados.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_04.png" alt="" title="Figura 4 - Dados desbalanceados"/>
</div>
<div class="col three caption">
	Figura 4 - Dados desbalanceados
</div>
<br/>

Foi verificado como se comporta a distribuição da variável *Amount*, na qual podemos ver na Figura 5 que ela possui uma distribuição assimétrica à direita (*positive skewed*).

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_05.png" alt=""  title="Figura 5 - Distribuição Assimétrica de Amount"/>
</div>
<div class="col three caption">
	Figura 5 - Distribuição Assimétrica de Amount
</div>
<br/>

### **Visualização Exploratória**
<br/>

Nosso conjunto de dados para preservar a confidencialidade possui 28 variáveis transformadas via PCA, sem estarem com seus verdadeiros rótulos, devido a isso a criação de hipóteses sobre elas torna-se um desafio um pouco maior. A suposição é que devido esta transformação elas não possuam correlação entre si, uma vez que PCA cria uma ortogonalidade entre essas features, tal fato é comprovado no Mapa de Calor na Figura 6. As variáveis que conhecemos são o Montante (Amount), Tempo em segundos (Time) e a Classe (Class). Na Figura 7 podemos avaliar como se comportam essas três features entre si, não sendo possível notar nenhum padrão.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_06.png" alt="" title="Figura 6 - Mapa de Calor: Correlação entre as variáveis"/>
</div>
<div class="col three caption">
	Figura 6 - Mapa de Calor: Correlação entre as variáveis
</div>
<br/>
<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_07.png" alt="" title="Figura 7 - Montante vs Tempo por Classe"/>
</div>
<div class="col three caption">
	Figura 7 - Montante vs Tempo por Classe
</div>
<br/>

### **Algoritmos e Técnicas**
<br/>
Neste projeto testaremos uma série de algoritmos da biblioteca do sklearn, na intenção de optar pelo o que obtiver um melhor resultado nas análises. Os modelos utilizados neste estudo são:

* **Support Vector Machines (SVM)**: utiliza uma técnica chamada de kernel para transformar os dados e encontra um limite ideal entre as saídas possíveis. Possui uma alta acuracidade, não é sensível a não linearidade dos dados, baixo risco de overfitting usando kernels corretos, trabalha bem com dados dimensionais elevados.

* **Regressão Logística (RL)**:estima a probabilidade associada à ocorrência de um determinado evento devido algumas features. Possui alto grau de confiabilidade e não é necessário supor normalidade multicolinearidade.

* **Random Forest (RF)**: um método ensemble utilizado para construir um modelo baseado em múltiplas Decision Trees durante a fase de treino. Possui baixo risco de overfitting, ele costuma ser rápido para treinar.

* **XGBoost (XGB)**: é uma implementação avançada do Gradient Boosting,  mais rápido e com alto poder preditivo. Possui uma variedade de regularizações que reduzem o *overfitting* e melhoram o desempenho geral.

As técnicas utilizadas neste projeto são:

* **GridSearch**: : otimiza os hiperparametros dos modelos para encontrar o modelo mais otimizado possível.

* **Under-sampling**: retira da modelagem linhas da classe majoritária, para evitar desbalanceamento.

* **Over-sampling**: replica linhas da classe minoritária ou gera dados sintéticos, para evitar desbalanceamento.

* **Synthetic Minority Over-Sampling Technique (SMOTE)**: Utilizado para sintetizar os dados no *oversampling*.  Tal técnica será implementada junto com *Tomek links* para evitar o aumento de variância causada por algumas técnicas de geração de dados sintéticos.

* **Log Transformation**: utilizado para normalizar os dados com distribuição assimétrica.

* **MinMaxScaler**: padroniza as *features* na mesma escala.

### **Benchmark**
<br/>
Utilizando o mesmo conjunto de dados no estudo  mais votado no Kaggle, o autor utilizou uma Regressão Logística para criar o modelo que obteve uma acurácia e recall próximo de 93% e, conforme a Figura 8, obtendo o 0,95 de ROC AUC.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_08.png" alt="" title="Figura 8 - Benchmark"/>
</div>
<div class="col three caption">
	Figura 8 - Benchmark
</div>
<br/>

### **Pré-Processamento**
<br/>

Devido a feature Amount ter uma distribuição assimétrica, com alguns dados sendo 0, efetuei uma transformação em log(x+1) e então passei para o ***Feature Scaling***, que consiste em reescalonar os dados para obter uma distribuição com média de zero e desvio padrão de um. A técnica utilizada para esta etapa foi o ***MinMaxScaler*** do *sklearn*, na coluna feature pelo log, tornando os dados em uma escala de 0 a 1. Foi criado um novo dataset para armazenar os dados originais mais o transformado e foi retirado as features *Time* e *Amount* original.

Uma vez que temos os dados prontos para analisar, faz-se necessário dividir os dados entre conjuntos de treinamento e de teste, em uma proporção de 70% para treinamento e 30% para teste. Por fim, para tratar o desbalanceamento dos dados foi necessário reamostrá-los, utiliznado somente os dados de treinamento, nesta etapa foi criada seis novos conjuntos de dados com diferentes técnicas de reamostragem, como podemos observar na Tabela 2. 


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_13_pt.jpg" alt="" title="Tabela 2 - Técnicas de Reamostragem"/>
</div>
<div class="col three caption">
	Tabela 2 - Técnicas de Reamostragem
</div>
<br/>

### **Implementação**
<br/>

Nesta etapa foi criada uma função para aplicar um algoritmo classificador sem nenhum parâmetro otimizado e já mostrar os resultados das métricas citadas no capítulo **Métricas de avaliação** e o tempo que o algoritmo usa para treinar e prever os dados, baseados nos datasets resultantes de reamostragem. O intuito desta etapa foi verificar qual o algoritmo e técnica de reamostragem que tem melhor performance nos dados de treinamento e teste para encontrarmos o melhor modelo. 

Na Tabela 3 estão todos os algoritmos separados por técnicas de reamostragem, mostrando o desempenho de cada modelo ordenado pelo valor mais alto em ROC AUC, sendo a métrica como principal influenciadora na decisão da escolha do modelo, seguido por f1_score para classe 1 e depois classe 0.

Podemos concluir observando a Tabela 3 que o algoritmo RL unido com a técnica de reamostragem Smote with Tomek Links com balanceamento de classes iguais, obteve a melhor pontuação de ROC AUC (0,947), contudo o f1_score para classe 1 foi muito baixo, sendo 0,110, portanto o algoritmo escolhido foi o que desempenhou os melhores resultados balanceados entre as duas métricas, sendo o algoritmo RF com a mesma técnica de reamostragem do modelo acima, obtendo o ROC AUC em 0,905 e f1_score em 0,850. 	

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_14.jpg" alt="" title="Tabela 3 - Performance dos Modelos"/>
</div>
<div class="col three caption">
	Tabela 3 - Performance dos Modelos
</div>
<br/>

### **Refinamento**
<br/>

Tendo o modelo e técnica de reamostragem definidas o passo seguinte foi a otimização do algoritmo, para isso foi utilizado a biblioteca GridSearch do sklearn. Aqui se faz importante ressaltar que uma vez que nossa métrica de avaliação será ROC AUC, tendo um algoritmo escolhido, utilizaremos a predição dele com probabilidade, que no caso do RF é predict_proba, isso porque como explicado no capítulo de **Métricas de avaliação**, a curva ROC AUC é feita variando o threshold (limite) para encontrar as taxas de falso positivo/negativo, calculando as probabilidades para cada classe

O resultado da ROC AUC com predict_proba no modelo sem refinamento foi 0.9439, com os seguintes hiperparametros:

```python
RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini',
            max_depth=None, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=1, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators='warn', n_jobs=None,
            oob_score=False, random_state=96, verbose=0, warm_start=False)
```

To improve the performance of the model GridSearch was used to optimize the hyperparameters according to Table 4.
<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_15_pt.jpg" alt="" title="Tabela 4 - Descrição de Hiperparâmetros"/>
</div>
<div class="col three caption">
	Tabela 4 - Descrição de Hiperparâmetros
</div>
<br/>

Com a otimização do modelo feito no passo anterior, o resultado final da ROC AUC com predict_proba no modelo otimizado foi 0.9820, podendo ser verificado na Figura 9, com os seguintes hiperparametros:

```python
RandomForestClassifier(bootstrap=True, class_weight=None, criterion='gini',
            max_depth=10, max_features='auto', max_leaf_nodes=None,
            min_impurity_decrease=0.0, min_impurity_split=None,
            min_samples_leaf=5, min_samples_split=2,
            min_weight_fraction_leaf=0.0, n_estimators=911, n_jobs=None,
            oob_score=False, random_state=96, verbose=0, warm_start=False)
```

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_09.png" alt="" title="Figura 9 - ROC AUC modelo otimizado"/>
</div>
<div class="col three caption">
	Figura 9 - ROC AUC modelo otimizado
</div>
<br/>

Ao verificar a relevância de cada feature no modelo o resultado foi que 5 atributos acumulavam 70% da importância dos dados, como podemos ver na Figura 10. Contudo ao utilizar o mesmo modelo com apenas as 5 features o ROC AUC caiu para 0.9182, devido a grande diferença da métrica de ambos modelos, foi escolhido o modelo otimizado com todas as features.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_10.png" alt="" title="Figura 10 - Importância das Features"/>
</div>
<div class="col three caption">
	Figura 10 - Importância das Features
</div>
<br/>

### **Conclusão**
<br/>

Acredito que o projeto obteve sucesso na análise de detecção de fraudes por cartão de crédito, conseguindo melhorar um pouco o resultado demonstrado no benchmark. Foram utilizadas várias técnicas na qual necessitaram muita pesquisa para seu entendimento e utilização. Outro ponto importante foi a necessidade de encontrar um modelo que tivesse um bom balanceamento na predição de ambas as classes, pois dos quatro algoritmos selecionados apenas o RF teve um bom desempenho (acima de 0,8) nas duas classes. Para um próximo passo seria ideal tentar melhorar a predição do modelo para os Falsos Positivos, sem piorar o desempenho dos Verdadeiros Positivos, uma vez que neste estudo tivemos 72 casos de Falsos positivos, vistos na Figura 11

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_11.png" alt="" title="Figure 11 - Confusion Matrix"/>
</div>
<div class="col three caption">
	Figure 11 - Confusion Matrix
</div>
<br/>


### **Bibliografia**
<br/>

Braga, Antonio Padua & Castro, Cristiano Leite de. 2011. Aprendizado Supervisionado com conjunto de dados desbalanceados

Castle, Nikki. 2017. Supervised vs. Unsupervised Machine Learning

Chen, Edwin. Choosing a Machine Learning Classifier

Donges, Niklas. 2018. The Random Forest Algorithm 

Google Developers. 2018. Classification: Precision and Recall

Google Developers. 2018. Classification: ROC and AUC 

Barella, Victor Hugo. 2015. Técnicas para o problema de dados desbalanceados em classificação hierárquica

Portella, Letícia.2018. Machine Learning Models - My Cheat List

Sachanm Lalit, 2015. Logistic Regression vs Decision Trees vs SVM: Part II

Statinfer, 2017. SVM : Advantages Disadvantages and Applications

Simplilean. 2018. KNN Algorithm

Singh, Aishwarya. 2018. A Comprehensive Guide to Ensemble Learning 

Souza, Jocelyn D’. 2018.  Let’s learn about AUC ROC Curve! 

### **PS:**
Este é o meu projeto de conclusão para Nanodegree em Engenheiro de Aprendizado de Máquina da Udacity, por favor me avise se tiver alguma dúvida ou sugestão. Todo o código que você encontrá no meu Github [aqui.](https://github.com/shiguelita/credit_card_fraud_detection). 

Obrigada! Até! ^^