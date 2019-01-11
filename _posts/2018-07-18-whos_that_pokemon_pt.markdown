---
layout: post
title: Quem é esse Pokémon?
date: 2018-07-19 18:30:00
comments: true
author: Talita Shiguemoto
lang: pt
ref: pokemonTypes
---

# Uma análise de visualização dos tipos Pokémon

<br/>
Para o meu primeiro estudo, gostaria de começar com algo de que eu realmente gosto: Pokémon!
Se você é um entusiasta de Pokémon como eu, você já pensou: "Eu teria um ginásio de *[insira seu tipo favorito]*!". E no meu caso, seria um tipo de Luta ou Fogo.
Eu fico imaginando quantos Pokémon de cada tipo existem? As opções de Fogo são menores que as de Luta?
Para responder a essas perguntas, escolhi esta breve análise. 

## Conjunto de Dados

<br/>
Os dados são fornecidos pelo Gurarako e você pode obtê-los [aqui.](https://www.kaggle.com/gurarako/basic-pandas-pokemon-dataset-part-1/data)
Este conjunto de dados é completo, com todos os 807 Pokémon (7 gerações). Aqui estão incluídas todas as evoluções, assim como o Mega Evolutions.
Meu primeiro passo foi preparar os dados, usando o Python no Jupyter Notebook. Eu importei a biblioteca Pandas para preparar os dataframes e para a visualização de dados eu usei as bibliotecas Bokeh, Matplotlib e Seaborn.

```python
#todas as bibliotecas importadas
import pandas as pd #dataframes
#usado nos graficos de barra
from bokeh.io import show,output_notebook, output_file
from bokeh.models import HoverTool, ColumnDataSource
from bokeh.plotting import figure 
from bokeh.core.properties import value
#usado no mapa de calor
import matplotlib.pyplot as plt 
import seaborn as sns
from math import pi

#faz os gráficos aparecerem no jupyter notebook
%matplotlib inline 
output_notebook() 
```

Uma vez importado o banco de dados, foi necessário fazer algumas alterações devido a erros de digitação. As colunas **Tipo 1** e **Tipo 2** tinham maiúsculas ou minúsculas. Eu converti todas as letras para maiúsculas e as ordenei em ascendente.


```python
#usando Pandas para importar meu arquivo csv e criar uma tabela de dados
df = pd.read_csv('database/pokemonall.csv')
#tornar todas as linhas maiúsculas
df['Type 1'] = df['Type 1'].str.upper()
df['Type 2'] = df['Type 2'].str.upper()
#ordenar por ascendente
firstType = df['Type 1'].value_counts().sort_index(ascending=True)
secondType = df['Type 2'].value_counts().sort_index(ascending=True)
```

O resultado do meu dataframe foi:


|   | # |      Name| Type 1| Type 2| Total| HP| Attack| Defense| Sp. Atk| Sp. Def| Speed| Generation| Legendary|
|--:|--:|---------:|------:|------:|-----:| -:|------:|-------:|-------:|-------:|-----:|----------:|---------:|
| 0 | 1 | Bulbasaur|  GRASS| POISON|   318| 45|     49|      49|      65|      65|    45|          1|     False|
| 1 | 2 |   Ivysaur|  GRASS| POISON|   405| 60|     62|      63|      80|      80|    80|          1|     False|




## Gráficos: Barra

<br/>
O primeiro gráfico nos diz quantos Pokémon existem para cada tipo, aqui são consideradas todas as evoluções e também, se um Pokémon tiver dois tipos, é contado duas vezes. Neste gráfico, podemos ver o valor do tipo por ordem, como por exemplo temos Bulbasauro, é principalmente Grass (Grama) e seu tipo secundário é Poison (Venenoso). Para este gráfico eu usei o Bokeh e podemos passa o mouse por cima do gráfico para obter o número de Pokémon para cada tipo, confira:

{% include bokeh_post02_types_count_by_order.html %}
<br/>
#### Script

```python
#criando um gráfico de barras com Bokeh
types = list(firstType.keys())
order = ["Primary", "Secondary"]
colors = ["#3182bd", "#9ecae1"]

source = ColumnDataSource(data = {'Types' : types,
        'Primary'   : list(firstType),
        'Secondary'   : list(secondType)})

hover=HoverTool(
     tooltips = [
        ('Primary', '@Primary'),
        ('Secondary', '@Secondary'),
        ('Type', '@Types'),
    ]
)

p = figure(x_range=types, plot_height=400, title="Types Counts by Order",
           toolbar_location=None, tools=[hover])

p.vbar_stack(order, x='Types', width=0.9, color=colors, source=source,
             legend=[value(x) for x in order])

p.y_range.start = 0
p.x_range.range_padding = 0.1
p.xgrid.grid_line_color = None
p.axis.minor_tick_line_color = None
p.outline_line_color = None
p.xaxis.major_label_orientation = pi/2
p.background_fill_color = None
p.legend.location = "top_left"
p.legend.orientation = "vertical"
p.legend.spacing = 10
p.legend.padding = 20
p.legend.margin = 20
```

Olhando para este gráfico, devo dizer, eu fiquei bastante impressionada com o número pequeno de tipos primários de Flying(Voador), é o terceiro maior grupo de Pokémon, nós temos muitos deles nos céus, mas apenas 4 são primários! Isso me faz pensar: Quantos Pokémon Voadores são puros (um tipo)?
Para saber que eu plotei este segundo gráfico:

{% include bokeh_post02_one_type_pokemon_count.html %}
<br/>

Para fazê-lo, tive que localizar todas as linhas com espaços em branco na coluna **Tipo 2** e colocá-las em uma *Series* do Pandas.

```python
indexNull = df[df['Type 2'].isnull()].index.tolist()
df2 = df.loc[indexNull,['Type 1']]
pureType = df2['Type 1'].value_counts().sort_index(ascending=True)
```

Depois, eu também usei o Bokeh para criar este gráfico:


```python
#criando um gráfico de barras com Bokeh
source = ColumnDataSource(data={
        'Types' : list(pureType.keys()),
        'Primary':list(pureType),
})


hover=HoverTool(
     tooltips = [
        ('Count', '@Primary'),
        ('Type', '@Types'),
    ]
)

p = figure(x_range=list(pureType.keys()), plot_height=400, 
title="One-Type Pokémon Count", toolbar_location=None, 
tools=[hover])

p.y_range.start = 0
p.x_range.range_padding = 0.1
p.xgrid.grid_line_color = None
p.outline_line_color = None
p.xaxis.major_label_orientation = pi/2
p.background_fill_color = '#fdfdfd'
p.border_fill_color = "#fdfdfd"

p.vbar(x='Types', width=0.9, top='Primary',color="#3182bd", source=source)
```

## Gráficos: Mapa de Calor

<br/>
Por fim, queria descobrir como é a interação cruzada entre os tipos primário e secundário, então escolhi um mapa de calor para visualizar essas informações.

Utilizando o método `.crosstab` eu manipulei o dataframe para cruzar todos os tipos 1 e 2, então eu criei dois totais com soma de colunas e linhas.

```python
#criando o novo Dataframe cruzando os tipos 1 e 2
crosstab_df = pd.crosstab(df['Type 1'], df['Type 2'])
#adicionando uma coluna total
crosstab_df['Total'] = crosstab_df.sum(axis=1)
#adicionando uma linha total
finaltable = crosstab_df.append(pd.DataFrame(crosstab_df.sum(),
columns=['Total']).T, ignore_index=False)
#verificando se funcionou
finaltable.tail()
```

Com meus dados organizados, usei Matplotlib e Seaborn para criar o último gráfico. Podemos notar que o maior grupo de Pokémon com os dois tipos são Normal com Voador. É meio que esperado, uma vez que Voador tem um número maior de tipos secundários, Water (Água) tem cerca de metade dos Pokémon com apenas um tipo e Normal sendo o segundo maior grupo de Tipo, com a maioria de seus Pokémon sendo primários.

```python
#criando um mapa de calor
sns.set(font_scale=1)
plt.figure(figsize=(14,7))
sns.heatmap(finaltable, cmap="Blues", annot=True, 
            vmax=finaltable.loc[:'WATER', :'WATER'].values.max(),
            vmin=finaltable.loc[:,  :'WATER'].values.min(), fmt='d')
plt.xlabel('Secondary type')
plt.ylabel('Primary type')
plt.title('Pokémon amount per primary and secondary type')
plt.savefig('post002_whos_that_pokemon.png', dpi=1000, 
            transparent=True, bbox_inches='tight', edgecolor=None)
```

<div style="display: flex; justify-content: center;">
<img  class="img-responsive" src="/img/notebook/post002_whos_that_pokemon.png" alt="" title="Pokémon amount per primary and secondary type"/>
</div>
<br/>

Dahora, né? Espero que tenha gostado!

Você pode verificar todas as análises no meu Github [aqui.](https://github.com/shiguelita/whos_that_pokemon)
