---
layout: post
title: Consulta Rápida: Modelos de Aprendizagem de Máquina
date: 2019-01-15 14:00:00
comments: true
author: Talita Shiguemoto
lang: pt
ref: csML
---

Esse é um resumo dos modelos e métrica que aprendi no Nanodegree Engenheiro de Machine Learning da Udacity, na qual contei minha experiência [aqui](https://shiguelita.github.io/posts/2019-01-10-my_experience_with_udacity_pt/). Acrescentei alguns modelos e bibliotécas que utilizei em outros momentos. Como é uma consulta rápida o objetivo será sempre atualizar este post.

Lembre-se isso não é um post com detalhes de cada modelos ou métricas, apenas para ter uma lembrança rápida, com breve resumo do modelo, vantagens, desvantagens e exemplos de casos de uso. Coloquei todas as importações no final, uma vez que normalmente utilizamos mais que uma em uma análise.

* [Aprendizagem Supervisionada](#supervised-learning)
	* [Naive Bayes](#naive-bayes)
	* [Árvores de Decisão](#decision-trees)
	* [Support Vector Machines](#svm)
	* [Regressão Linear](#linear-regression)
	* [K-Nearest Neighbors](#knn)
	* [Regressão Logística](#logistic-regression)
	* [Métodos de Ensemble](#ensemble)
	* [Métricas de Validação](#evaluating-sl)
* [Aprendizagem Não Supervisionada](#unsupervised-learning)
	* [KMeans](#kmeans)
	* [Hierárquico](#hierarchical)
	* [DBSAC](#dbsac)
	* [Modelos de Mistura de Gaussianas](#gmm)
	* [Métricas de Validação](#evaluating-ul)
* [Feature Scaling](#feature-scaling)
* [Principal Component Analysis](#pca)
* [Projeções Aleatórias](#random-projection)
* [Independent Component Analysis](#ica)
* [Importando bibliotécas](#import)

## **Aprendizagem Supervisionada** {#supervised-learning}
<br/>
Aprendizagem Supervisionada são modelos de Machine Learninig que baseados em dados históricos, com entradas e saídas que já sabemos como corretas, o algoritimo é treinado para conseguir mapear novos resultados.

Existem dois tipos de problemas para esse tipo de aprendizagem:
   * **Regressão**: tentamos prever resultados quantitativos (numéricos)
   * **Classificação**: tentamos prever resultados qualitativos (discretos)

### **Naive Bayes** {#naive-bayes}
<br/>

**O que é?**

É um classificar probabilíticos baseado no *Teorema de Bayes*, criado por Thomas Bayes. Contudo, por ser ingênuo ele pode ter falsos positivos.

**Teorema de Bayes**: probabilidade de um evento ocorrer condicionado por outro.
*Exemplo*: Se *A* acontecer a probabilidade de *B* acontecer é *XX%*.

**Naive**: entende que todas as ocorrências são independentes, o que pode gerar falsos positivos.

Existem 3 tipos de classificadores de Naives Bayes:
* **Bernoulli Naive Bayes**: assume que os features são binários, normalmente representados por 0 sendo negativo e 1 positivo.
* **Multinomial Naive Bayes**: usado com os features são discretos, como palavras ou classificações de 1 a 5. 
* **Gaussian Naive Bayes**: usado quando os features são contínuos e há a suposição da distribuição normal. 

* **Onde pode ser usado?**
    * Filtro de SPAM
    * Análise de Sentimento
    * Análise de perfil: em uma análise de perfil de emprestímo,  verifica se um usuário é um bom pagador, se paga assiduamente, entre outras variáveis, para concluir se é rentável o emprestímo para tal usuário
    * Mineração de dados: banco de dados com vários textos para identificar estilo literário ou autor, baseado no histórico, como estrutura do texto, linguagem, palavras usadas
    * Existe um sistema que apoia decisões média a respeito de doenças cardiológicas, o [Heart Disease Prediction System](https://pdfs.semanticscholar.org/d32e/e90a5de89093a4fc95f43e0409cb91414726.pdf)(Sistema de Previsão de Doenças Cardíacas). Ele foi desenvolvido usando como modelo princial o **Naive Bayes**. O modelo consegue responder a consultas complexas para diagnosticar doenças cardíacas e, assim, ajudar   profissionais de saúde tomarem decisões clínicas inteligentes que os sistemas tradicionais de apoio à decisão não podem
    * O modelo de Gaussian Naives Bayes foi utilizado para predição de inibição de atividade protéica por pequenas moléculas de absorção, distribuição, metabolismo e excreção (ADME), priorizando os acertos de campanhas de triagem de alta produtividade e enriquecimento dos resultados do acoplamento de alto rendimento

* **Vantagens**
    * Fácil implementação
    * Consegue trabalhar com dados categóricos e continuos
    * É performático e pode ser usado em real time
    * Não necessita de um conjunto de dados de treinamento tão grande comparado a outros modelos
    * Não é sensível a features irrelevantes
    * Retorna o grau de certeza da resposta

* **Desvantagens**
    * Devido ele não identificar a interação dos dados, sua dependência, pode ocorrer falsos positivos
    * Quando tratamos com frases existe o problema dele separar cada palavra e há perda de sentido como Chicago Bulls, sendo um time de basquete e o algorítimo interpresta *Chicago* como cidade e Bulls como Touro 

### **Árvores de Decisão** {#decision-trees}
<br/>

**O que é?**

Árvore de Decisão é um diagrama em forma de árvore usado para determinar um curso de ação. Cada ramo da árvore representa uma possível decisão, ocorrência ou reação.

Uma Árvore de Decisão pode ser usada de duas formas:
* **Classificador**: usado para prever valores quando a variável dependente (Y) é categórica.
* **Regressão**: usado para prever valores quando a variável dependente (Y) é contínua.

* **Onde pode ser usado?**
    * Detecção de Fraude em cartões de crédito, como pode ser visto [nesse exemplo](https://www.ijcsmc.com/docs/papers/April2015/V4I4201511.pdf)
    * Sistemas de Recomendações: como produtos, filmes, sites, etc. Um caso real foi o [Recommender Systems (RS)](https://subs.emis.de/LNI/Proceedings/Proceedings165/170.pdf), um algorítmo criado com objetivo de aumentar as vendas e a satisfação dos clientes, ele tenta prever a classificação que um usuário dará aos itens com base em suas classificações anteriores e nas classificações de outros usuários com perfil semelhante e assim recomenda o item para compra.

* **Vantagens**
    * Fácil de entender, interpretar e visualizar
    * Necessita de pouco esforço para preparar os dados (não precisa normalizar os dados, etc)
    * Pode ser usado com dados categóricos e númericos
    * Não necessita preocupação com outliers ou se os dados não são linearmente
    * Lida facilmente com interações de features e não são paramétricos
    
* **Desvantagens**
    * Não suporta aprendizado online, precisa reconstruir a árvore quando obtiver novos dados
    * Pode facilmente ser sobreajustado (overfitting), contudo pode-se contornar isso com Ensemble Methods
    * O modelo pode ficar instável devido à pequena variação nos dados
    * Estabilidade: pequenas variações nos dados podem gerar a árvores muitos diferentes
    * Pode ser tendenciosa caso alguma classe seja dominante

### **Support Vector Machines** {#svm}
<br/>

**O que é?**

Em português *Máquina de Vetores de Suporte* pode ser usado para problemas de classificação e regressão. Ele usa uma técnica chamada de *kernel*  para transformar seus dados e, em seguida, com base nessas transformações, ele encontra um limite ideal entre as saídas possíveis.

* **Onde pode ser usado?**
    * Reconhecimento facial
    * Reconhecimento de caligrafia
    * Previsão de Estrutura de Proteína
    * Diagnóstico de Câncer de Mama: os resultados experimentais mostram que kernel linear baseados no método *bagging* podem ser as melhores escolhas para um conjunto de dados de pequena escala, onde a seleção de recursos deve ser realizada no estágio de pré-processamento de dados. Para um conjunto de dados em grande escala, o kernel RBF baseados com o método *boosting* tivevem melhor desempenho de aprimoramento que os outros classificadores, veja [nesse exemplo](https://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0161501&type=printable)    
    

* **Vantagens**
    * Alta acuracidade
    * Pode trabalhar com dados não lineares
    * Funciona bem com dados ainda não estruturados e semi-estruturados, como texto, imagens e árvores.
    * Com uma função de kernel apropriada, pode-se resolver  problemas complexos
    * Trabalha bem com dados dimensionais elevados
    * Baixo risco de overfitting, usando os kernels corretos
    

* **Desvantagens**
    * Não fornece estimativas de probabilidade diretamente
    * Não performa bem com grandes conjuntos de dados de treinamento
    * Difícil em escolher uma boa função kernel
    * Difícil de entender e interpretar o modelo final, pesos variáveis e impacto individual.

### **Regressão Linear** {#linear-regression}
<br/>

**O que é?**

Regressão Linear é utilizado para prever valores contínuos de saída (Y) baseado em uma ou mais variáveis preditoras de entrada (X). Ela estabelece uma relação linear com os valores de X conhecidos para prever o valor de Y.

* **Regressão Linear Simples**: Quando temos apenas uma variável de entrada (X)
* **Regressão Linear Múltipla**: Quando temos apenas duas ou mais variáveis de entrada (X)

* **Onde pode ser usado?**
    * Prever o salário anual em função do tempo de experiência em uma determinada empresa
    * Prever Vendas vs Valor investido para marketing
    * Prever Ganho de Peso diário vs Peso de entrada
    * Prever Aquisão de cartões de rédito vs Limite do cartão, beneficíos de multiplus
    * Prever Produção vs Quantidade de Exportação e Capacidade
    * Prever Crescimento econômico
    
* **Vantagens**
    * Fácil de entender e explicar
    * Identifica o quanto o modelo consegue explicar os valores observados (R²)
    * Consegue trabalhar com um conjunto de dados pequeno    
    
* **Desvantagens**
    * Necessita que a variável de saída tenha distribuição normal
    * Dificilmente o R² é alto para casos reais    

* **Conceitos**
    * SSE (Sum Square Errors): Soma dos Erros Quadrados, quanto mais baixo melhor, usado para comparar outros modelos como métrica de ajuste
    * Degree of Freedom: Graus de liberade, número de valores no cálculo final de uma estatística que estão livres para variar
    * R²: um coeficiente de determinação que varia entre 0 e 1 e mostra o quanto o modelo consegue explicar os valores observados. Quanto maior o R², mais explicativo é o modelo, melhor ele se ajusta à amostra.

### **K-Nearest Neighbors** {#knn}
<br/>

**O que é?**

Estima a probabilidade associada à ocorrência de um determinado evento devido algumas *features*. Os resultados da análise ficam contidos em um intervalo de 0 a 1.

* **Onde pode ser usado?**
    * Precisão de risco na área tributária.  
    * Utilizado para classificar se a empresa encontra-se no grupo de empresas solvente ou insolvente.
    * Determinar quais características levam as empresas adotarem o *balanced scorecard*.
    * [Todos exemplos acima citados aqui](https://edisciplinas.usp.br/pluginfile.php/3769787/mod_resource/content/1/09_RegressaoLogistica.pdf)
    
* **Vantagens**
    * Fornece resultado em termos de probabilidade
    * Alto grau de confiabilidade
    * Não é necessário supor multicolinearidade
    * Modelos podem ser facilmente atualizados com novos dados
  
* **Desvantagens**
    * Sensível a outliers
    * Não perfoma bem com alta dimensionalidade

### **Regressão Logística** {#logistic-regression}
<br/>

**O que é?**

Algoritmo que estima a probabilidade associada à ocorrência de um determinado evento devido algumas features. Sua variável saída necessita ser contínua e de entradas discretas.

* **Onde pode ser usado?**
    * Segmentação de imagem e categorização
	* Processamento de imagens geográficas
	* Reconhecimento de caligrafia
	* Detecção de spam

* **Vantagens**
    * Pode ser atualizado com facilidade em dados novos
    * Não precisa se preocupar com a multicolinearidade dos dados
    * Além de ser um classificador também retorna probabilidade, porcentagem do evento poder ocorrer ou não

* **Desvantagens**
    * Não trabalha bem com outliers
    * Não trabalha bem um grande número de variáveis categóricas

### **Métodos de Ensemble** {#ensemble}
<br/>

**O que é?**

Métodos de Ensemble são formas de combinar vários modelos, criando um modelo mais forte e preciso.

Alguns dos métodos são:
* **Random Florest**: utilizado para construir um modelo baseado em múltiplas Árvores de Decisão durante a fase de treino.

* **AdaBoost**: melhora o modelo por meio dos erros, alterando os pesos.

* **Bagging**: treina em vários subconjuntos, ou seja, separa os dados de treino e os divide aleatoriamente, cada um tendo um *"weak learner"*, então uni os resultados dos *"weak learner"* para criar um modelo mais forte com base nas saídas mais vistas pelos modelos

* **Gradient Boosting**: tenta minimizar o Erro Médio Quadrático (EQM, ou MSE em inglês).até que a soma dos resíduos seja próxima de 0 (ou mínimo) e os valores previstos estejam suficientemente próximos dos valores reais. 

* **XGBoost (Extreme Gradient Boosting)**: é uma implementação avançada do Gradient Boosting, contudo mais rápido e com alto poder preditivo. Possui uma variedade de regularizações que reduzem o overfitting e melhoram o desempenho geral, como penalização inteligente de árvores e um encolhimento proporcional de nós de folha.

* **Onde pode ser usado?**
    * Random Florest usado no Kinect para rastreiar movimentos corporais e os recriar no jogo.
    * AdaBoosting foi usado para detectar a análise de jogos de jogadores de beisebol, veja aqui a [documentação.](https://www.uni-obuda.hu/journal/Markoski_Ivankovic_Ratgeber_Pecev_Glusac_57.pdf)


* **Vantagens**
    * Melhor em evitar o overfitting
    * Normalmente tem modelos mais fortes poder preditivo e acuracidade que modelos únicos
    
* **Desvantagens**
    * Escalonamento: geralmente por treinar vários modelos, pode ter um desempenho ruim com grandes conjuntos de dados
    * Difícil de implementar em plataforma de tempo real

### **Métricas de Validação** {#evaluating-sl}
<br/>

Siglas usadas nos conceitos abaixo:
* TP: True Positive, são os Verdadeiros Positivos, valores que foram previstos corretamente pelo algoritmo como positivos
* FP: False Positive, são os Falsos Positivos, valores que foram previstos incorretamente como positivos
* TN: True Negatives, são os Verdadeiros Negativos, valores que foram previstos corretamente como negativos
* FN: False Negatives, são os Falsos Negativos, valores que foram previstos incorretamente como negativos

* **Acurácia**: mede com que frequência o classificador faz a predição correta. É a proporção entre o número de predições corretas e o número total de predições (o número de registros testados). Matematicamente expressado por:

\begin{align}
\dot{Acurácia} & = 	\frac{TP+TN}{TP+TN+FP+FN} 
\end{align}

* **Matriz de Confusão**: em inglês Confusion Matrix, é uma matriz de valores reais e valores preditos pelo seu algoritmo, mostrando quantos TP, TN, FP e FN seu modelo obteve. Como podemos ver na Figura 1:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_11.png" alt="" width="400px" height="320px" title="Figura 1 - Confusion Matrix"/>
</div>
<div class="col three caption">
	Figura 1 - Confusion Matrix
</div>
<br/>

* **Precisão**: é a proporção entre os Verdadeiros Positivos sobre todas os positivos, sendo eles classificados corretamente ou não. Matematicamente representado por:

\begin{align}
\dot{Precision} & = 	\frac{TP}{TP+FP} 
\end{align}

* **Recall**: também chamado de **True Positive Rate (TPR)** ou **Sensibilidade**, é a proporção entre os Verdadeiros Positivos sobre todas os positivos classificados corretamente e os falsos negativos. Matematicamente representado por:

\begin{align}
\dot{Recall} & = 	\frac{TP}{TP+FN} 
\end{align}

* **ROC**: uma curva ROC traça TPR vs. False Positive Rate (FPR) em diferentes limiares de classificação. Diminuir o limiar de classificação classifica mais itens como positivos, aumentando assim tanto os falsos positivos quanto os verdadeiros positivos. Em outras palavras, um modelo prediz a probabilidade de uma classe ser 1 ou 0, usando essas probabilidades é possível plotar um gráfico de distribuição como na figura abaixo, sendo a curva vermelha representando 0 e a verde para 1 , sendo 0,5 o limite entre as duas classes.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project001_01.png" alt="" title="Figura 2 - ROC"/>
</div>
<div class="col three caption">
	Figura 2 - ROC
</div>
<br/>

Todos os valores positivos acima do limite (maiores que 0.5) serão True Positives (TP), e todos os valores negativos acima do limite serão False Positives (FP), uma vez que foram classificados incorretamente como positivos. Abaixo do limite, todos os valores negativos serão True Negatives (TN) e os positivos False Negatives (FN), uma vez que foram classificados incorretamente como negativos. Esse conceito é melhor demonstrado na figura a seguir.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_02.png" alt="" align="center" title="Figura 3 - TN, TP, FN, FP"/>
</div>
<div class="col three caption">
	Figura 3 - TN, TP, FN, FP
</div>
<br/>

* **AUC**: a curva AUC mede toda a área bidimensional por baixo de toda curva ROC. AUC fornece uma medida agregada de desempenho em todos os possíveis limites de classificação. Uma maneira de interpretar AUC é como a probabilidade de o modelo classificar um exemplo positivo aleatório mais do que um exemplo negativo aleatório. Um modelo cujas previsões são 100% erradas tem uma AUC de 0,0; enquanto um cujas previsões são 100% corretas tem uma AUC de 1,0. Conforme Figura 4.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project001_03.png" alt="" title="Figura 4 - ROC AUC"/>
</div>
<div class="col three caption">
	Figura 4 - ROC AUC
</div>
<br/>

* **Classification Report**: trata-se de uma tabela de reporte com as principais métricas de avaliação para cada classe, sendo elas: recall, precision e *f1_score*.

* **f1_score**: é uma medida que considera ambos: precision e recall, sendo sua fórmula expressada por:

\begin{align}
\dot{f1 score} & = 2 \cdot	\frac{precision \cdot recall}{precision + recall} 
\end{align}

* **fbeta_score**: calculado através da média(harmônica) dos valores de precision e de recall, tendo um parâmetro beta que determina o peso do precision no score, sendo `beta < 1` com maior peso para precision e `beta > 1` favorecendo recall(beta = 0 considera somente precision, beta = inf somente recall). Matematicamente expressado por:

\begin{align}
\dot{F_beta score} = (1 + \beta^2) \cdot \frac{precision \cdot recall}{(\beta^2 \cdot precision) + recall}
\end{align}


## **Aprendizagem Não Supervisionada** {#unsupervised-learning}
<br/>
Aprendizagem Não Supervisionadas são modelos de Machine Learnig que possuem poucos ou nenhum dado histórico para se basear e conseguir prever os resultados, eles não necessitam de um conjunto de dados que digam quais são as variáveis de saídas corretas, para modelar um algoritmo preditivo. Esses modelos conseguem criar estruturas de dados com base em relações entre as variáveis ou detectar algumas tendências.

### **KMeans** {#kmeans}
<br/>

**O que é?**

Um algoritmo que agrupa dados que possuem características similares entre si em um *cluster*. O método K-means tem esse nome porque *K* representa um número, uma vez que é necessário dizer ao algoritmo em quantos clusters deseja a divisão dos dados. K-means é mais utilizado em uso geral, com clusters de tamanhos iguais, geometria plana, sem grandes quantidades de clusters. O algoritmo sempre tenta encontrar clusters circulares, esféricos.

Ele usa a medida de distância para dizer o quão similiar cada observação é, podendo ser:
   * **Distância Euclidiana**: distância euclidiana entre dois pontos em uma linha reta
   * **Distância de Manhattan**: soma das distâncias entre as linhas que formam 90º entre dois pontos, ou seja, soma da linha y até o ponto de intersecção e a linha x. Essas medidas são sensíveis à diferença de escalas entre variáveis distintas:
   * **Distância do Cosseno**: medida do ângulo entre dois vetores. Se o ângulo é 0 existe total similariade e se é π não existe relação entre os objetos.


* **Onde pode ser usado?**
    * Performance acadêmica, baseado nas notas de estudantes para separar entre estudantes A, B ou C
    * Sistema de diagnóstico
    * Classificação de catálogo de filmes
    * Classificação de espécies de animais ou plantas
    * Segmentação de clientes de acordo com perfis de consumo


* **Vantagens**
    * Fácil implentação e interpretação
    * Bom quando já se possui uma ideia do número necessários de clusters
    * Boa escabilidade com muitas variáveis se o k não for tão alto


* **Desvantagens**
    * As *sementes (seeds)* inicias tem um grande impacto nos resultados finais devido o local minumm, quando são iniciados em locais não tão adequados. Dificuldade que pode ser contornada rodando o algorítmo muitas vezes
    * Difícil de prever o número de clusters (K-Value), pode-se usar *elbow method* para contornar isso
    * Sensível a outliers
    * Não trbaalha bem com missing values
    * Não trabalha bem com clusters que não são circulares ou esféricos
    
    
* **Comandos no Sklearn**     
    * n_clusters: número de clusters desejados, padrão 8 
    * max_iter: quantas intereções máximas entre atribuir os pontos e mover o centroide, padrão 300
    * n_init: quantas vezes o algoritmo muda os centroides para diferentes clusters, padrão 10
    
    
* **Como encontrar o número de clusters (k)**
    * **Elbow Method**: método utilizado para  encontrar o número de clusters mais adequados para cada conjunto de dados, ele plota os valores ascendentes de k versus o erro total calculado usando esse k. Abaixo um código para encontrar esse valor:

```python
df = pd.DataFrame(__)

k_values = range(2, len(df)+1, 5)

scores = []
for k in k_values:
    kmeans = KMeans(n_clusters=k).fit(df)
    prediction = kmeans.predict(df)
    score = silhouette_score(df, prediction)
    scores.append((k, score)) 
```
### **Hierárquico** {#hierarchical}
<br/>

**O que é?**

O resultado deste algoritmo é uma estrutura de agrupamento que nos proporciona uma indicação visual da relação entre os agrupamentos. Ele uni os dados em clusters de acordo com distâncias mínimas, que são calculadas de acordo com cada método:

* **Single Link**: assume que todos os pontos são um cluster e depois os agrupa em clusters de acordo com a distância dos pontos. Depois, quando há um ponto isolado, ele procura os dois pontos mais **próximos** nos dois clusters que é a distância entre os clusters, e e depois une os pontos formando outro cluster. E então escolhemos quantos clusters queremos. **Este método não existe no sklearn**, em vez disso usamos o ***Complete Link***
* **Complete Link**: assume que todos os pontos são um cluster e depois os agrupa em clusters de acordo com a distância dos pontos formando outro cluster. E então, quando há um ponto isolado, ele procura os dois pontos mais **afastados** nos dois clusters que é a distância entre os clusters, e e depois une os pontos. E então escolhemos quantos clusters queremos.
Para unir outros clusters, ele calcula a distância maior entre os pontos, que vamos nomear de distância X, depois mede a distância X menor entre clusters para uní-los.
O problema deste método é que somente um ponto é analisado para verificar a menor distância, sem levar em conta os outros dados, que podem estar unidos de forma mais densa, podendo ser outro cluster.
* **Average Link**: verifica a distância entre todos os pontos entre clusters e a média de todas as distâncias é a medida de distância entre os dois clusters.
* **Ward's Link**: método que tenta minimizar a variância ao unir os dois clusters. Ele calcula um ponto central entre dois clusters, depois verifica a distância entre cada ponto e o ponto central, os eleva ao quadrado e soma todas as distâncias. Depois ele calcula um ponto central em cada cluster, e subtrai cada distância entre pontos e ponto central de cada cluster, elevada ao quadrado. **Este é o método padrão do sklean.**


* **Onde pode ser usado?**
    * Usando o método de Average Link, foi possível agrupar proteínas produzidas, usado para criar clusters de tipos de fungos, veja na [documentação](https://www.ncbi.nlm.nih.gov/pubmed/22238666)
    * Usando o método Complete Link para desenhar um diagrama de microbioma humano, veja na [documentação](https://www.ncbi.nlm.nih.gov/pubmed/21129376)

* **Vantagens**
    * Funciona bem com conjunto de dados dois-crescentes e dois-anéis
    * Fácil visualização de cada clusters (dendogramas)
    * Especialmente potente quando o conjunto de dados contém relacionamento hierárquico real (EX:biologia evolutiva)
    * Representações hierárquicas resultantes são muito informativas


* **Desvantagens**
    * Necessita escolher qual método mais adequado
    * Sensível a ruídos e outilers
    * Precisa de capacidade computacional maior quando tiver grandes conjuntos de dados e muitas dimensionalidades

### **Density Based Spatial Clustering of Application with Noise** {#dbsac}
<br/>

**O que é?**

O algoritmo agrupa os dados que estão unidos de forma densa, pontos próximos juntos, e nem todos os pontos fazem parte de um cluster, o model nomeia os outros pontos como ruído.
Ele verifica a distância ε (epsilon) em volta dos pontos e caso não haja outros pontos próximos suficientes, é considerado ruído. Temos que setar os parametros de número mínimo de pontos por cluster e distância ε.
**Core point (ponto cetral)**: é o ponto que possui o número mínimo de pontos requeridos e identifica um cluster. Um mesmo cluster pode ter vários core points
**Border point (ponto marginal)**:pontos que não possuem o número mínimo de pontos requeridos em volta, contudo fazem parte de um cluster em volta de um Core Point 

* **Onde pode ser usado?**
    * Analisar tráfego de rede, um administrador de rede consegue separar por grupos os tipos de tráfegos como usuários de downloads massivos e outros. Veja a [documentação](https://conferences.sigcomm.org/sigcomm/2006/papers/minenet-01.pdf)
    * Detecção de anomalias em dados de temperatura, na qual os pontos nomeados como ruídos são as anomalias, outliers. Veja a  [documentação](https://ieeexplore.ieee.org/document/5946052)

* **Vantagens**
    * Bom com conjunto de dados com ruído ou outliers
    * Flexivel para trabalhar com formas e trabanhos diferentes de clusters
    * Não é necessário especificar o número de clusters
    * Funciona bem com conjunto de dados dois-crescentes e dois-anéis


* **Desvantagens**
    * Border points que podem ser acessados de dois clusters são atribuídos ao cluster que o encontra primeiro
    * Enfrenta dificuldade em encontrar clusters de diferentes densidades, podemos usar HDBSCAN nesses casos

### **Modelos de Mistura de Gaussianas** {#gmm}
<br/>

**O que é?**

Em inglês chamado de *Gaussian Mixture Models*, é um algoritmo de agrupamento leve na qual todo dado é parte de todos os clusters, contudo com níveis diferentes de pertencimento. Ele faz uma distribuição de dados, e se for gaussiana eles tem um nivel de agrupamento maior, ou seja, ele mistura as distribuições gaussianas para separar os clusters.

Ele funciona pelo algortimo de Maximização da Expectativa, nos seguintes passos:
* Definir K, número distribuições gaussianas. Setar média e variâncias   
* Passo E, agrupamento suave dos dados. calcula o grau de pertenciomento de cada ponto para cada cluster  
* Passo M, passo da maximização, usar os dados do passo anterior e criar novos paramettos de média e variância para cada cluster. de forma ponderada levando em conta o nível de pertencimetno de cada ponto  
* Validar o log-likelihood (log-verossimilhança) para checar se converge. Quanto maior o número mais certeza temos que o modelo de mistura que geramos é responsavel pela criação dos dados ou que encaixe no dataset que temos


* **Onde pode ser usado?**
    * Leitura de dados de sensores para encontrar rotinas de pessoas. Ex: Como um sensor de deslocamento, o algoritmo consegue identificar quando estão no escritório, jantando ou fora. Ou também, consegue diferenciar qual o meio de condução que a pessoa utilizada. Veja a [documentação](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.681.3152&rep=rep1&type=pdf)
    * Identificar fala, assinaturas, biometria.
    * Visão computacional, consegue detectar fundos de imagem, por exemplo tirando pessoas de uma vídeo e deixando apenas o cenário. Veja a [documentação](http://www.ai.mit.edu/projects/vsam/Publications/stauffer_cvpr98_track.pdf)


* **Vantagens**
    * Precisa de menos treino que outros modelos, como regressão logística
    * Alta escalabilidade
    * Não é sensível a *features* com pouca importância
    * Retorna o grau de certeza da resposta
    * Agrupamento leve, que faz com que amostras possam fazer parte de múltiplos clusters


* **Desvantagens**
    * Sensível aos valores de inicialização
    * A taxa de convergência é lenta
    * Não é bom em aprender com intereções de *features*, por exemplo, se vocês gosta de sorvete de flocos e chocolate, mas odeia os dois sabores juntos

### **Métricas de Validação** {#evaluating-ul}
<br/>

* **Índices Externos**: usado quando os dados originais estiverem rotulados.Um dos seus índices é o Adjusted Rand score, sendo de -1 a 1, quanto maior melhor.

* **Índices Internos**: usado para pedir o ajusto entre os dados e a estrutura. Um dos seus índices é o Silhouette score, sendo de -1 a 1, quanto maior melhor, não é um bom índice para DBSCAN, nem tão pouco para agrupamentos definidos como dois-anéis. Bom para agrupamentos compactos, densos, circulares. Para DBSCAN é bom utilizar o índice DBCV.

* **Índices Relativos**: indica qual estrutura de clusters é melhor em algum sentido

**Os índices de validação são definidos como:**
   * **Compactação**: medida de proximidade dos elementos em cada grupo
   * **Separabilidade**: medida de quão distantes os clusters estão uns dos outos

### **Feature Scaling** {#feature-scaling}
<br/>

**O que é?**

Método para transformar as features com intervalos semelhantes para que possam ser compáveis, normalmente entre 0 e 1.
No Sklearn existe um método que faz isso, o ***MinMaxScaler***, que transforma os dados nessa escala, contundo precisa lembrar que seu array tem que ter números *float* (decimais)

Exemplos de modelos que são afetados por Feature Scaling:
* K-Means
* SVM com RBF

Exemplos de modelos que **não** são afetados por Feature Scaling:
* Regressão Lienar
* Decision Trees

* **Vantagens**
    * Ter um número confiável em relação ao que você espera na saída    
    
* **Desvantagens**
    * Sensível a outilers

### **Principal Component Analysis** {#pca}
<br/>

**O que é?**

* Usado para reduzir a dimensionalidade dos *features* maximizando as variâncias, perdendo a menor quantidade de informação ao projetador os dados
* PCA é um modelo sistematizado de transformar as *features* de entradas em componentes principais.
* Esses Componentes Principais ficam disponíveis para se usar no lugar das *features* originais
* Pode-se rankear os Componentes Principais
* Os Componentes Principais devem ser perpendiculares entre si (90º), que significa que são independentes entre si
* O número máximo de Componentes Principais que se pode obter é igual ao número de inputs do conjunto de dados, contudo é recomendável não usar todos, e sim os que estiverem com maior raking

**Variáveis Mensuravéis**: pode medir de maneira direta, como m², número de quartos, ranking escolar

**Variáveis Latentes**: não podemos medir diretamente, mas é representada por outros indicadores (as mensuráveis), gerando um sentido teórico. Ex: tamanho é a variável latente e suas variáveis mensuráveis são m² e número de quartos.

* **Quando usar?**
    * Quando queremos ver se variáveis latentes que pode ser mostradas nos padrões dos dados
    * Diminuir a dimensionalidade para:
        * Visualizar quando temos grandes dimensionaldiades
        * Diminuir os ruídos   
        * Fazer um pré processamento dos dados
                
* **Onde pode ser usado?**
    * Reconhecimento facial junto com SVM

### **Projeções Aleatórias** {#random-projection}
<br/>

**O que é?**

Um método de redução de dimensionalidade, matematicamente mais eficaiz e perfomático que PCA para trabalhar com conjunto de dados com grande dimensionalidade. Baseado no Lema de Johnson-Linderstrauss, o algortimo multiplica o cojunto de dados por uma matriz aleatória, preservando as distancias entre os pontos em um grau amplo, fazendo com que o número de dimensões diminua

### **Independent Component Analysis** {#ica}
<br/>

* **O que é?**

ICA assume que os *features* são misturas de fontes independentes, e o algoritmo tenta isolá-las umas das outras.
Um exemplo conhecido é o "*Problema da festa*", que mostra três pessoas no mesmo salão em locais diferentes, cada uma gravando o som do local. Cada local está tocando algo difente, como piano, violão celo, e uma TV.

Todas as gravações captam tosdos os sons, mas de intensidade diferente para cada. Conseguir limpar as três gravações para conseguir o som de cada objeto sozinho é o que o ICA faz, sendo esse processo chamado de **Separação cega das fontes**
O algoritmo assume que os componentes são estatísticamentes **independentes** e com uma **distribuição não-gaussiana**

* **Onde pode ser usado?**
    * Scanners Médicos: mapeamento cerebral, sensores detectam sinais elétricos ou magnéticos. ICA pode ser usado para encontrar os componetes independentes dentro do cérebro.
    * Ressonâncias magnéticas para isolar de onde vem cada sinal do cérebro

### **Importando bibliotécas** {#import}
<br/>

```python
#aprendizagem supervisionada
from sklearn.naive_bayes import GaussianNB
from sklearn.naive_bayes import MultinomialNB
from sklearn.naive_bayes import BernoulliNB
from sklearn.tree import DecisionTreeClassifier
from sklearn.tree import DecisionTreeRegressor
from sklearn.svm import SVC
from sklearn.linear_model import LinearRegression
from sklearn.neighbors import KNeighborsClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.ensemble import RandomForestRegressor
from sklearn.ensemble import AdaBoostClassifier
from sklearn.ensemble import BaggingClassifier
from sklearn.ensemble import BaggingRegressor
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.ensemble import GradientBoostingRegressor
from xgboost.sklearn import XGBClassifier

#aprendizagem não supervisionada
from sklearn.cluster import KMeans
from sklearn.cluster import AgglomerativeClustering #Hierarquico
from sklearn.cluster import DBSCAN
from sklearn.mixture import GMM

#Feature Scaling
from sklearn.preprocessing import MinMaxScaler
from sklearn.preprocessing import StandardScaler
#PCA
from sklearn.decomposition import PCA
#Projeções Aleatórias
from sklearn import random_projection
#ICA
from sklearn.decomposition import FastICA

#métricas de avaliação
from sklearn.metrics import accuracy_score
from sklearn.metrics import confusion_matrix
from sklearn.metrics import recall_score
from sklearn.metrics import precision_score
from sklearn.metrics import f1_score
from sklearn.metrics import fbeta_score
from sklearn.metrics import classification_report
from sklearn.metrics import auc
from sklearn.metrics import roc_curve
from sklearn.metrics import roc_auc_score
from sklearn.metrics.cluster import adjusted_rand_score
from sklearn.metrics import silhouette_score

#outros
from sklearn.model_selection import train_test_split #separar dados em treino e teste
from IPython.display import display # Permite o uso de display() para DataFrames
from statsmodels.graphics.gofplots import qqplot #verificar normalidade
from scipy.stats import shapiro #verificar normalidade
from scipy.stats import normaltest #verificar normalidade
from sklearn.model_selection import GridSearchCV #tunar o modelo
from sklearn.metrics import make_scorer #usar no GridSearch
import seaborn as sns
import pandas as pd
import numpy as np
from time import time
```

## **Bibliografia**
<br/>

Castle, Nikki. 2017. [Supervised vs. Unsupervised Machine Learning](https://www.datascience.com/blog/supervised-and-unsupervised-machine-learning-algorithms)

Çelik, Mete, et al. 2001.[Anomaly detection in temperature data using DBSCAN algorithm](https://ieeexplore.ieee.org/document/5946052)

Chen, Edwin. [Choosing a Machine Learning Classifier](http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/)

Erman, Jefreey, et al. 2006. [Traffic Classification Using Clustering Algorithms](https://conferences.sigcomm.org/sigcomm/2006/papers/minenet-01.pdf)

Gershman, Amir, et al. [A Decision Tree Based Recommender System](https://subs.emis.de/LNI/Proceedings/Proceedings165/170.pdf)

Grover, Prince. 2017. [Gradient Boosting from scratch](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)

Huang, Min-Wei, et al. 2017. [SVM and SVM Ensembles in Breast Cancer
Prediction](https://journals.plos.org/plosone/article/file?id=10.1371/journal.pone.0161501&type=printable)

Klon, Anthony E., et al 2006. [Improved Naive Bayesian Modeling of Numerical Data for Absorption, Distribution, Metabolism and Excretion (ADME) Property Prediction](https://pubs.acs.org/doi/pdf/10.1021/ci0601315)

Markoski,Branko. 2015. [Application of AdaBoost Algorithm in
Basketball Player Detection](https://www.uni-obuda.hu/journal/Markoski_Ivankovic_Ratgeber_Pecev_Glusac_57.pdf)

Pattekari,Shadab Adam & Parveen, Asma. 2012. [PREDICTION SYSTEM FOR HEART DISEASE USING NAIVE BAYES](https://pdfs.semanticscholar.org/d32e/e90a5de89093a4fc95f43e0409cb91414726.pdf)

Portella, Letícia. 2018. [Machine Learning Models - My Cheat List](https://leportella.com/cheatlist/2018/05/20/models-cheat-list.html)

[Regressão Logística](https://edisciplinas.usp.br/pluginfile.php/3769787/mod_resource/content/1/09_RegressaoLogistica.pdf)

Sachanm Lalit, 2015. [Logistic Regression vs Decision Trees vs SVM: Part II](https://www.edvancer.in/logistic-regression-vs-decision-trees-vs-svm-part2/)

Saunders, Diane, et al. 2012. [Using hierarchical clustering of secreted protein families to classify and rank candidate effectors of rust fungi](https://www.ncbi.nlm.nih.gov/pubmed/22238666)

Simplilean. 2018. [Naive Bayes Classifier](https://www.slideshare.net/Simplilearn/naive-bayes-classifier-naive-bayes-algorithm-naive-bayes-classifier-with-example-simplilearn)

Simplilean. 2018. [KNN Algorithm](https://goo.gl/XP6xcp)

Singh, Aishwarya. 2018. [A Comprehensive Guide to Ensemble Learning (with Python codes)](https://www.analyticsvidhya.com/blog/2018/06/comprehensive-guide-for-ensemble-models/)

Sonagara, Darshan and Badheka, Soham. 2014. [Comparison of Basic Clustering Algorithms](https://pdfs.semanticscholar.org/f0a4/d6bfb37b6c1102f7ef6b0d0f2ef861da6aca.pdf)

Spencer, Melanie, et al. 2010. [Association between composition of the human gastrointestinal microbiome and development of fatty liver with choline deficiency](https://www.ncbi.nlm.nih.gov/pubmed/21129376)

Statinfer, 2017. [SVM : Advantages Disadvantages and Applications](https://statinfer.com/204-6-8-svm-advantages-disadvantages-applications/)

Stauffer, Chris and Grmson W, E, L. [Adaptive background mixture models for real-time tracking](http://www.ai.mit.edu/projects/vsam/Publications/stauffer_cvpr98_track.pdf)

Sun, Feng-Tso, et al. [Nonparametric Discovery of Human Routines from Sensor Data](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.681.3152&rep=rep1&type=pdf)

[Sklearn documentation on Clustering Agglomerative](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.AgglomerativeClustering.html)

[Sklearn documentation on Clustering K-means](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)

[Sklearn documentation on Clustering DBSCAN](http://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html)

[Sklearn documentation on Decision Trees](http://scikit-learn.org/stable/modules/tree.html)

[Sklearn documentation on Naive Bayes](http://scikit-learn.org/stable/modules/naive_bayes.html) 

[What is the difference between the the Gaussian, Bernoulli, Multinomial and the regular Naive Bayes algorithms?](https://www.quora.com/What-is-the-difference-between-the-the-Gaussian-Bernoulli-Multinomial-and-the-regular-Naive-Bayes-algorithms)