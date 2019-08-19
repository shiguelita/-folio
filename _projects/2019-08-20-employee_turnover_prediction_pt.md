---
layout: post
title: Prevendo a rotatividade dos funcionários - Parte 1
date: 2019-08-20 13:00:00
author: Talita Shiguemoto
description: Análise exploratória e modelagem estatística dos dados de rotatividade
comments: true
ref: employeeTurnover
lang: pt
---

Segundo o site da Forbes (2018) em uma pesquisa realizada pela BambooHR nos Estados Unidos da América, 17% dos trabalhadores deixam o emprego nos primeiros seis meses de contratação dizem que um “um sorriso de um colega de trabalho poderia ter feito toda a diferença” (Forbes, 2018), outros 23% dizem que uma comunicação mais efetiva com o gerente poderia ter mudado a decisão de sair da empresa e mais 21% culpam um treinamento ineficiente. 

O objetivo deste projeto é analisar estatísticamente quais são as possíveis razões do desligamento espontâneio e criar um algoritmo para detectar quais profissionais podem ter a rotatividade positiva. Com tais informações uma empresa tem a possibilidade de entender as razões dos índices de desligamento, sendo possível então criar ações para prevenir os casos por ela indesejados.

Este projeto será divido em duas partes, sendo esta primeira referente a análise exploratórias dos dados, na qual será levantada hipóteses que serão testadas estatísticamente. Todo procedimento será feito na linguagem R.

A segunda parte será a criação do modelo de machine learning para prever os colaboradores com rotatividade positiva. Nesta parte a análise será feita na linguagem python.


### **Descrição da base de dados e das variáveis**
<br/>

O conjunto de dados é referente a informações fictícias de rotatividade de funcionários criado pelos cientistas de dados da IBM, composto por 1.470 observações e 35 variáveis. Os dados podem ser obtidos no [Kaggle](https://github.com/shiguelita/IBM_HR_analysis).

#### **Variáveis qualitativas**

**Nominais**

- Attrition: demonstra se colaborador saiu da empresa por espontânea vontade. 
- Department: departamento do colaborador. 
- EducationField: área de educação. 
- EmployeeNumber: código do funcionário.
- Gender: gênero. 
- JobRole: cargo. 
- MaritalStatus: estado civil. 
- Over18: acima de 18 anos.
- OverTime: se faz hora extra.

**Ordinais**

- BusinessTravel: nível de frequência que o colaborador viajava a trabalho. 
- Education: escolaridade.
- EnvironmentSatisfaction: nível de satisfação com o ambiente de trabalho.
- JobInvolvement: nível de envolvimento com o trabalho. 
- JobLevel: nível de hierarquia do cargo.
- JobSatisfaction: nível de satisfação com o trabalho.
- PerformanceRating: nível de performance do colaborador.
- RelationshipSatisfaction: nível de satisfação com o relacionamento entre colaboradores.
- StockOptionLevel: nível de compra de ações da empresa.
- WorkLifeBalance: nível de equilíbrio entre a vida pessoal e no trabalho.

#### **Variáveis quantitativas**

**Contínuas**

- Dailyrate: valor da diária paga.
- DistanceFromHome: distância do trabalho até a casa.
- HourlyRate: valor da hora paga.
- MonthlyIncome: renda mensal.
- MonthlyRate: subsídio mensal.
- PercentSalaryHike: percentual de aumento de salário.
- StandardHours: Número de horas padrão trabalhadas.


**Discretas**

- Age: idade do colaborador.
- EmployeeCount: contagem de funcionários na observação.
- NumCompaniesWorked: número de empresas que já trabalhou.
- TotalWorkingYears: total de anos trabalhados.
- TrainingTimesLastYear: total de treinamentos no último ano.
- YearsAtCompany: total de anos na empresa.
- YearsInCurrentRole: total de anos no cargo atual.
- YearsSinceLastPromotion: total de anos desde a última promoção.
- YearsWithCurrManager: total de anos com o gestor atual.


### **Análise Exploratória dos Dados (EDA)**
<br/>

O primeiro item verificado foi se existia algum missing value no conjunto de dados. Como podemos ver na **Gráfico 1**, não existe nenhuma feature com a presença de missing values.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project002_01_pt.png" alt="" title="Gráfico 1 - Verificação de Missing Values"/>
</div>
<div class="col three caption">
	Gráfico 1 - Verificação de Missing Values
</div>
<br/>

Por meio da análise descritiva de todos as variáveis quantitativas, visto na **Tabela 1**, pode-se concluir que:
1. Não há nenhum menor de idade na base de dados, visto que a idade mínima é 18 anos;
2. As variáveis “EmployeeCount” e “StandardHours” tem um desvio padrão de 0, em outras palavras, não há nenhuma variação, todas as observações destas features são iguais (1 e 80 respectivamente), assim podemos retirar estas variáveis das análises.
3. As variáveis “DailyRate”, “DistanceFromHome“, “MonthlyRate”, “NumCompaniesWorked” e  “PercentSalaryHike” estão com um coeficiente de variação acima de 25%, sendo as features com maior variabilidade, em uma futura análise iremos verificar se esta variação se mantem entre as pessoas com “Attrition” positivo e negativo.

<div style="display: flex; justify-content: center;">
<img class="img-responsive" src="/img/projects/project002_02.png" alt="" title="Tabela 1. Estatística descritiva"/>
</div>
<div class="col three caption">
	Tabela 1. Estatística descritiva
</div>
<br/>

Para conseguir prever quais são os colaboradores com rotatividade, precisamos detectar as classes da feature “Attrition”, no **Gráfico 2** podemos notar que a diferença da frequência relativa entre duas classes é alta, sendo 83,9% para “No” e 16,1% para “Yes”, tal comportamento é algo que as empresas desejam, sendo a resposta positiva a menor possível.  

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_03.png" alt=""  title="Gráfico 2 - Balanceamento da variável Attrition"/>
</div>
<div class="col three caption">
	Gráfico 2 - Balanceamento da variável Attrition
</div>
<br/>

A tendência do mercado para grandes cidades é que o trabalhador busca por oportunidades mais perto de sua casa, uma hipótese levantada seria se a distância do trabalho a casa interfere na rotatividade da empresa. Podemos notar no **Gráfico 3** que quando a rotatividade é positiva a distância no terceiro quartil está maior em aproximadamente 5 unidades, tal suposição será testada posteriormente.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_04.png" alt="" title="Gráfico 3 - Boxplot Distância de casa por Rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 3 - Boxplot Distância de casa por Rotatividade
</div>
<br/>

Ao analisarmos o **Gráfico 4** percebemos que para rotatividades positivas, 50% das pessoas estão no primeiro emprego, o que gera a hipótese que pessoas que saem da empresa tem em média o número de empregos anteriores menor.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_05.png" alt="" title="Gráfico 4 - Boxplot Nº empresas trabalhadas por Rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 4 - Boxplot Nº empresas trabalhadas por Rotatividade
</div>
<br/>

Outra hipótese que surge, ao explorarmos a renda mensal pela rotatividade no Gráfico 5 é que quem recebe menos tem maior tendência de se desligar da companhia, uma vez que os somente 50% colaboradores que permanecem ganham até 5000 enquanto quase 75% dos que saem recebem o mesmo valor.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_06.png" alt="" title="Gráfico 5 - Boxplot Renda mensal pela Rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 5 - Boxplot Renda mensal pela Rotatividade
</div>
<br/>

A maioria das variáveis ordinais são classificadas por meio de numeração, para facilitar a análise exploratória dos dados foi documentada a descrição de cada uma nas tabelas a seguir, mostrando também a frequência absoluta e relativa que cada classe aparece.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_07.png" alt="" title="Tabela 2 – Tabela de frequência Education"/>
</div>
<div class="col three caption">
	Tabela 2 – Tabela de frequência Education
</div>
<br/>

Para entender melhor o universo dos dados analisados, vemos na **Tabela 2** que a maior concentração de colaboradores está no nível educacional de Bacharel seguido de Mestrado enquanto analisando a **Tabela 3**, vemos que a área de educação da maior parte dos funcionários é em “Life Sciences” e “Medical”.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_08.png" alt="" title="Tabela 3 - Tabela de frequência EducationField"/>
</div>
<div class="col three caption">
	Tabela 3 - Tabela de frequência EducationField
</div>
<br/>

Quando analisamos o nível de satisfação dos colaboradores, existem três dimensões capturadas no conjunto de dados, sendo elas: satisfação com o ambiente de trabalho (EnvironmentSatisfaction), satisfação com o trabalho (JobSatisfaction) e satisfação com o relacionamento entre colaboradores (RelationshipSatisfaction). Na **Tabela 4** mostrados a descrição dos níveis de satisfação, sendo 1 o mais baixo e 4 o mais alto. Observando a frequência das classes é possível notar que o comportamento entre as três variáveis se apresenta semelhante. 

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_09.png" alt="" title="Tabela 4 - Tabela de frequência variáveis de satisfação"/>
</div>
<div class="col three caption">
	Tabela 4 - Tabela de frequência variáveis de satisfação
</div>
<br/>

Quando analisamos o nível de satisfação dos colaboradores, existem três dimensões capturadas no conjunto de dados, sendo elas: satisfação com o ambiente de trabalho (EnvironmentSatisfaction), satisfação com o trabalho (JobSatisfaction) e satisfação com o relacionamento entre colaboradores (RelationshipSatisfaction). Na **Tabela 4** mostrados a descrição dos níveis de satisfação, sendo 1 o mais baixo e 4 o mais alto. Observando a frequência das classes é possível notar que o comportamento entre as três variáveis se apresenta semelhante. 

Ao analisarmos os níveis de satisfação com a rotatividade de trabalho na **Figura 1** pode-se perceber que a classe com a menor satisfação possui uma maior frequência de “Yes” em todos os gráficos apresentados, tal análise exploratória faz surgir a hipótese de que se existe algum diferença estatística entre os níveis de satisfação do colaborador e a rotatividade. Esta possibilidade será tratada na modelagem estatística.

Ao entrarmos mais a fundo nas variáveis que referenciam como os colaboradores se envolvem com o trabalho, existem três que de fato mostram esse cenário: nível do cargo (JobLevel), envolvimento com o trabalho (JobInvolvement) e a taxa de performance do colaborador (PerformanceRating). Tais features estão descritas na **Tabela 5**.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_10.png" alt="" title="Figura 1 - Gráficos de Satisfação vs Rotatividade"/>
</div>
<div class="col three caption">
	Figura 1 - Gráficos de Satisfação vs Rotatividade
</div>
<br/>

Pela frequência apresentada e nos níveis de cargos da **Tabela 5**, podemos concluir que 73% dos colaboradores estão nos cargos mais baixos, a performance apresenta apenas duas classes, sendo elas excelente e fora do padrão, em outras palavras, classes boas e por fim que 69% dos funcionários apresentam um envolvimento com o trabalho alto ou muito alto.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_11.png" alt="" title="Tabela 5 - Frequência de variáveis sobre cargo e comportamento"/>
</div>
<div class="col three caption">
	Tabela 5 - Frequência de variáveis sobre cargo e comportamento
</div>
<br/>

Cargos com níveis maiores costumam receber mais, tal ponto também é visto no conjunto de dados, demonstrado no **Gráfico 6**. O cargo mais alto (5) é o que possui maior renda mensal e é ocupado por pessoas com a idade mais alta, começando aproximadamente com 40 anos.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_12.png" alt="" title="Gráfico 6 - Renda mensal vs Idade por Nível de Cargo"/>
</div>
<div class="col three caption">
	Gráfico 6 - Renda mensal vs Idade por Nível de Cargo
</div>
<br/>

Mesmo o nível de performance sendo alto para os dois níveis, a maior parte dos colaboradores tem o nível 3, alto, sendo 85% do total. Muitas empresas tem como política o aumento de salário maior para as pessoas com o maior desempenho, ao verificarmos o **Gráfico 7** notamos que os dados estudados também segue esse padrão, sendo o percentual de aumento salarial consideravelmente maior para o nível de performance 4.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_13.png" alt="" title="Gráfico 7 - Boxplot Performance vs Aumento salarial (%)"/>
</div>
<div class="col three caption">
	Gráfico 7 - Boxplot Performance vs Aumento salarial (%)
</div>
<br/>

Ao explorarmos como é a relação entre vida pessoal e trabalho, na variável “WorkLifeBalance”, na **Tabela 6**, pode-se perceber que poucos são os que tem melhor nível de balanceamento, contudo 61% estão na segunda melhor classe. Na mesma tabela vemos que a maior parte dos colaboradores não fazem hora extra e quase não viajam a trabalho.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_14.png" alt="" title="Tabela 6 - Frequência vida pessoal, viagens e hora extra"/>
</div>
<div class="col three caption">
	Tabela 6 - Frequência vida pessoal, viagens e hora extra
</div>
<br/>

Ao analisarmos qual a frequência dos colaboradores que fazem hora extra pela rotatividade no **Gráfico 8**, é possível perceber mais de 50% dos que se desligam tinha como positivo se trabalhavam mais horas, gerando a hipótese de que fazer hora extra pode impactar na rotatividade.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_15.png" alt="" title="Gráfico 8 - Frequência se faz Hora Extra vs Rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 8 - Frequência se faz Hora Extra vs Rotatividade
</div>
<br/>

Na **Tabela 7** temos a frequência por departamento, sendo que a maioria está em Research & Development (Pesquisa e desenvolvimento) e mesmo tendo no conjunto de dados apenas três áreas de atuação, há uma diversidade de cargos.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_16.png" alt="" title="Tabela 7 - Frequência Departamento e Cargo"/>
</div>
<div class="col three caption">
	Tabela 7 - Frequência Departamento e Cargo
</div>
<br/>

A **Tabela 8** apresenta dados relacionados aos indivíduos como pessoas. O primeiro item ressaltado é que todos os colaboradores são maiores de 18 anos, fato já confirmado na **Tabela 1**, sendo assim podemos retirar esta variável da análise. Outro ponto interessante é que há um certo balanceamento de gênero, não possuindo uma diferença muito grande entre homens e mulheres. O estado civil tem em sua maioria, 46% de pessoas casadas e referente a possibilidade de compra de ações, 43% estão no nível 0, ou seja, sem direito de compra, e outros 41% podendo comprar apenas o nível mais básico de ações.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_17.png" alt="" title="Tabela 8 - Frequência de Gênero, Estado Civil, Maioridade e Opção de compras de ações"/>
</div>
<div class="col three caption">
	Tabela 8 - Frequência de Gênero, Estado Civil, Maioridade e Opção de compras de ações
</div>
<br/>


### **Modelagem Estatística**
<br/>

Por meio da análise exploratória dos dados, visto no Capítulo 4, cinco hipóteses foram levantadas:
1. A média da distância entre trabalho e casa é estatisticamente igual entre colaboradores com rotatividade positiva e negativa?
2. Existe diferença estatística nos níveis de satisfação médio (com o ambiente, trabalho ou relacionamento) entre o grupo de funcionários com rotatividade positiva e negativa?
3. A média das empresas trabalhadas anteriormente é estatisticamente igual entre funcionários que deixam ou não a companhia?
4. Entre colaboradores com rotatividade há diferença estatística na renda mensal média?
5. Os funcionários que fazem hora extra têm maior chance estatística de deixarem a empresa?

O presente capítulo apresentará o teste não paramétrico de Mann-Whitney U para as hipóteses 1, 2, 3 e 4 e o teste do Qui-quadrado de Pearson e Razão de Chances para a hipótese 5.

#### **Análise da normalidade das distribuições**

Para escolher qual método estatístico seria utilizado, foi necessário em primeiro lugar verificar a distribuição dos dados, se a suposição de normalidade era verdade. Para isso os passos seguidos foram como sugere do autor Pino (2014):

> > “Para verificar se a distribuição é normal, a primeira coisa a fazer é um gráfico de frequências das observações, para examinar se existem assimetrias. Ao se desconfiar da existência de não normalidade, o passo seguinte é testar a hipótese nula de que a distribuição das observações é normal, contra a hipótese alternativa de que não o é. Chama-se teste de aderência (ou de ajustamento) o problema de testar a hipótese de que uma dada amostra provém de uma população com uma função densidade específica.” 

Como podemos analisar no **Gráfico 9** e no **Gráfico 10**, as variáveis dependentes deste estudo não possuem uma distribuição normal, sendo que no primeiro gráfico, todas as features são ordinais, enquanto no gráfico seguinte os histogramas possuem um formato assimétrico positivo. 


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_18.png" alt="" title="Gráfico 9 - Distribuição variáveis de satisfação"/>
</div>
<div class="col three caption">
	Gráfico 9 - Distribuição variáveis de satisfação
</div>
<br/>

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_19.png" alt="" title="Gráfico 10 - Distribuição variáveis"/>
</div>
<div class="col three caption">
	Gráfico 10 - Distribuição variáveis
</div>
<br/>

Já o teste de aderência utilizado foi o teste de Shapiro-Wilk, que segundo Pino (2014), é um teste baseado em regressão e correlação, que usa a razão de uma estimativa ponderada de mínimos quadrados e outra estimativa sem viés de variância amostral. Tal teste tem como equação:

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_20.png" alt="" title="Equação Shapiro-Wilk"/>
</div>
<div class="col three caption">
	Equação Shapiro-Wilk
</div>
<br/>

sendo c'= (c1,...cn) e V são respectivamente, segundo Pino (2014), vetores de valores esperados e a matriz de covariâncias das estatísticas de ordem da norma padrão.
Os resultados do teste de Shapiro-Wilk para as 6 variáveis podem ser vistos na **Tabela 9**, sendo que a Hipótese Nula (H_0) de que a distribuição é normal é rejeitada para todos os testes, devido ao p-valor<0,05.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_21.png" alt="" title="Tabela 9 - Resultados Shapiro-Wilk Teste"/>
</div>
<div class="col three caption">
	Tabela 9 - Resultados Shapiro-Wilk Teste
</div>
<br/>


É possível transformar os dados para que eles obtenham uma distribuição normal, uma vez que tais transformações “podem melhorar a aproximação à normalidade das funções de verossimilhança e, também, a precisão de níveis de significância e dos intervalos de confiança baseados na teoria para grandes amostras” (Pino, 2014).
Como todas as variáveis analisadas são positivas, a transformação escolhida foi a logarítmica, sendo sua equação:

\begin{align}
\dot{Y} & = 	{log⁡〖(x+c)}
\end{align}

sendo que se o valor mínimo apresentado for 0, c será igual a 1, caso contrário, c = 0.

Mesmo após a transformação, quando plotados os histogramas, de acordo com o **Gráfico 11** e Gráfico **12**, os dados não apresentam normalidade em sua distribuição

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_22.png" alt="" title="Equação Shapiro-Wilk"/>
</div>
<div class="col three caption">
	Equação Shapiro-Wilk
</div>
<br/>


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_23.png" alt="" title="Gráfico 11 - Histograma variáveis de satisfação em log"/>
</div>
<div class="col three caption">
	Gráfico 11 - Histograma variáveis de satisfação em log
</div>
<br/>


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_24.png" alt="" title="Gráfico 12 - Histograma variáveis em log"/>
</div>
<div class="col three caption">
	Gráfico 12 - Histograma variáveis em log
</div>
<br/>



Os resultados do teste de Shapiro-Wilk para as 6 variáveis transformadas em log podem ser vistos na Tabela 10, sendo que a Hipótese Nula (Ho) de que a distribuição é normal é rejeitada para todos os testes, devido ao p-valor<0,05.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_25.png" alt="" title="Tabela 10 - Resultados Shapiro-Wilk Teste em log"/>
</div>
<div class="col three caption">
	Tabela 10 - Resultados Shapiro-Wilk Teste em log
</div>
<br/>

#### **Mann-Whitney U teste**

O teste de Mann-Whitnet U de acordo com Milenovic (2011) é uma técnica estatística não paramétrica, usada para analisar se existe diferença entre as medianas de duas amostras independentes, para quando a distribuição da variável resposta não é normal, que invalida o uso do teste T de Student.
Para poder aplicar o teste de Mann-Whitnet U existem quatro pressuposições que precisam ser atendidas, que segundo Laerd (2013) são:
1. A variável dependente precisa ser ordinal ou contínua;
2. A variável independente precisa ser categórica, com duas classes independentes;
3. As observações precisam ser independentes entre as classes, em outras palavras, não pode haver relacionamento entre as observações de cada grupo testado, como por exemplo deve haver diferentes pessoas em cada grupo;
4. A distribuição dos dados não é normal e deve possuir o mesmo formato, como no **Figura 2**.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_26.png" alt="" title="Figura 2 - Forma das distribuições"/>
</div>
<div class="col three caption">
	Figura 2 - Forma das distribuições
	<br/>
	<a href="https://statistics.laerd.com/spss-tutorials/mann-whitney-u-test-using-spss-statistics.php">Fonte: Laerd, 2013</a>

</div>
<br/>

Como foi verificado no subcapítulo anterior, as variáveis dependentes não possuem distribuição normal, sendo RelationshipSatisfaction, JobSatisfaction e EnvironmentSatisfaction ordinais e DistanceFromHome, NumCompaniesWorked e MonthlyIncome contínuas. 
Os testes de hipóteses serão efetuados tendo a variável independente a rotatividade (Attrition), portanto todas as observações são independentes entre as classes, bem como cada classe é independente entre si.
A última suposição, de que as distribuições precisam ter o mesmo formato, foi comprovada por meio do **Gráfico 13** e **Gráfico 14**.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_27.png" alt="" title="Gráfico 13 - Distribuição por Rotatividade das variáveis de satisfação"/>
</div>
<div class="col three caption">
	Gráfico 13 - Distribuição por Rotatividade das variáveis de satisfação
</div>
<br/>


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_28.png" alt="" title="Gráfico 14 - Distribuição por Rotatividade das variáveis contínuas"/>
</div>
<div class="col three caption">
	Gráfico 14 - Distribuição por Rotatividade das variáveis contínuas
</div>
<br/>

Uma vez que todas as pressuposições foram atendidas, foi aplicado o teste de Mann-Whitney U para todas as hipóteses, com os resultados mostrados na Tabela 11.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_29.png" alt="" title="Tabela 11  - Resultados do teste de Mann-Whitney U"/>
</div>
<div class="col three caption">
	Tabela 11  - Resultados do teste de Mann-Whitney U
</div>
<br/>

A hipótese nula (Ho) de que a satisfação com o relacionamento entre os colaboradores (RelationshipSatisfaction) é estatisticamente o mesmo em ambos os grupos de rotatividade é aceito, uma vez que rejeitamos a hipótese alternativa (Ha) devido ao p-valor=0,102, ou seja, maior que 0.05. 
Nas outras duas categorias de satisfação, com o trabalho (JobSatisfaction) e com o ambiente de trabalho (EnvironmentSatisfaction), o p-valor de ambas é 0,000, assim é rejeitada a hipótese nula (Ho) e o modelo é significante. No **Gráfico 15** vemos que a média de satisfação para os que tem rotatividade negativa é maior em ambos os casos.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_30.png" alt="" title="EGráfico 15 - Média de satisfação entre Rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 15 - Média de satisfação entre Rotatividade
</div>
<br/>


O modelo que verifica se a quantidade de empresas trabalhadas anteriormente (NumCompaniesWorked) é estatisticamente diferente entre os grupos não é significante, tendo o p-valor=0,242 rejeitamos a hipótese alternativa (Ha) em favor da hipótese nula (Ho).
A hipótese nula (Ho) de que a distância da casa até o trabalho (DistanceFromHome) é igual estatisticamente entre os colaboradores que se desligam da empresa ou não é rejeitada devido o p-valor no teste resultar em 0,002, ou seja, p-valor<0,05, sendo o modelo significante. Ao analisarmos a média de renda mensal (MonthlyIncome) entre os dois grupos, por meio do p-valor<0,05 é rejeitada a hipótese nula, mostrando que há significância no modelo e que os grupos com rotatividade positiva e negativa tem a renda mensal diferente. 
No **Gráfico 16** é possível notar que o grupo de rotatividade positiva mora mais longe em média que o outro grupo e recebem uma renda mensal média menor.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_31.png" alt="" title="Gráfico 16 - Médias entre os grupos de rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 16 - Médias entre os grupos de rotatividade
</div>
<br/>

O modelo que verifica se a quantidade de empresas trabalhadas anteriormente (NumCompaniesWorked) é estatisticamente diferente entre os grupos não é significante, tendo o p-valor=0,242 rejeitamos a hipótese alternativa (Ha) em favor da hipótese nula (Ho).
A hipótese nula (Ho) de que a distância da casa até o trabalho (DistanceFromHome) é igual estatisticamente entre os colaboradores que se desligam da empresa ou não é rejeitada devido o p-valor no teste resultar em 0,002, ou seja, p-valor<0,05, sendo o modelo significante. Ao analisarmos a média de renda mensal (MonthlyIncome) entre os dois grupos, por meio do p-valor<0,05 é rejeitada a hipótese nula, mostrando que há significância no modelo e que os grupos com rotatividade positiva e negativa tem a renda mensal diferente. 
No **Gráfico 16** é possível notar que o grupo de rotatividade positiva mora mais longe em média que o outro grupo e recebem uma renda mensal média menor.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_32.png" alt="" title="Gráfico 16 - Médias entre os grupos de rotatividade"/>
</div>
<div class="col three caption">
	Gráfico 16 - Médias entre os grupos de rotatividade
</div>
<br/>

#### **Teste Qui-Quadrado de Pearson e Razão de Chances**


O teste Qui-Quadrado de Pearson é um teste não paramétrico que segundo a autora McHugh (2013), é um dos mais poderosos testes de hipóteses para duas variáveis qualitativas, que se utiliza uma tabela de contingência. As pressuposições do teste Qui-Quadrado de Pearson que necessitam ser atendidas, que segundo McHugh (2013) são:
1. As variáveis dependentes e independentes precisam ser qualitativas;
2. As observações precisam ser independentes entre os grupos, bem como os tais grupos precisam ser independentes entre si;
3. Todas as células da tabela de contingência precisam ter uma contagem mínima de 1;
4. Todas as células esperadas precisam ter uma contagem mínima de 5;
	* Caso esta pressuposição não for atendida, pode ser usado o teste de Fishers.
A última hipótese a ser testada é se os funcionários que fazem hora extra têm maior chance de se desligarem da empresa. Sendo as hipóteses:
- Ho : a chance de a rotatividade ser positiva por fazer hora extra é igual a chance da rotatividade negativa
- Ha : a chance de a rotatividade ser positiva por fazer hora extra é diferente da chance da rotatividade negativa
Ambas as variáveis são nominais, ambas possuindo o valor Sim (Yes) e Não (No), o que atende a suposição número 1. Todos os grupos como observações deles são independentes, atendendo a pressuposição 2.  A terceira suposição é atendida na **Tabela 12**, na qual nenhuma célula possui valor menor que 1.


<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_33.png" alt="" title="Tabela 12 - Tabela de Contingência"/>
</div>
<div class="col three caption">
	Tabela 12 - Tabela de Contingência
</div>
<br/>

Ao aplicarmos o teste Qui-Quadrado de Pearson, os resultados podem ser vistos na **Tabela 13**. Devido ao p-valor<0,05 a hipótese nula é rejeitada, sendo o modelo significante e temos a evidência que há diferença estatística entre os grupos que fazem hora extra e deixam a empresa.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_34.png" alt="" title="Tabela 13 - Resultado Teste de Qui-Quadrado de Pearson"/>
</div>
<div class="col three caption">
	Tabela 13 - Resultado Teste de Qui-Quadrado de Pearson
</div>
<br/>

A **Tabela 14** mostra a tabela de contingência dos valores esperados, sendo que nenhuma célula possui valor menor que cinco, assim o modelo atende a última pressuposição descrita por McHugh (2013), podendo então confiar na saúde do modelo.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_35.png" alt="" title="Tabela 14 - Tabela de Contingência dos valores esperados"/>
</div>
<div class="col three caption">
	Tabela 14 - Tabela de Contingência dos valores esperados
</div>
<br/>

Uma vez que podemos afirmar que há diferença estatística entre os grupos, para responder à pergunta de qual a chance dos colaboradores que fazem hora extra deixarem a companhia comparada ao outro grupo, precisamos utilizar o teste chamado Odds Ratio (OR).
A Razão de Chances, também conhecida como Odds Ratio é, segundo Colosimo (2006), a chance de um evento ocorrer entre as observações expostas a um fator, dividido pela chance desse mesmo evento ocorrer entre as observações não expostas a tal fator. Sua fórmula representada por:

\begin{align}
\dot{OR} & = 	\frac{P(A|B)/P(A'|B)}{P(A|B')/P(A'|B')} 
\end{align}

sendo A = evento ocorrer, A’ = evento não correr, B = exposto ao fator e B’ = não exposto ao fator.
A **Tabela 15** mostra o resultado do teste de Razão de Chances, na qual se pode concluir que a chance do colaborador que faz hora extra deixar a empresa é 3,77 vezes maior que a chance do colaborador que não faz hora extra.

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_36.png" alt="" title="Tabela 15 - Resultados Razão de Chances"/>
</div>
<div class="col three caption">
	Tabela 15 - Resultados Razão de Chances
</div>
<br/>

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/projects/project002_3.png" alt="" title="Equação Shapiro-Wilk"/>
</div>
<div class="col three caption">
	Equação Shapiro-Wilk
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