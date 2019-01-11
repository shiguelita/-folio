---
layout: post
title: Who's that Pokémon?
date: 2018-07-19 18:30:00
comments: true
author: Talita Shiguemoto
lang: en
ref: pokemonTypes
---

# A data visualization analysis of Pokémon Types

<br/>
For my first study I would like to start with something I really like: Pokémon!
If you are a Pokémon enthusiast as I am, you already thought: "I'd have a *[insert your favorite type]*-gym!". And in my case, it would be a Fighting or Fire type.
I was wordering, how many Pokémon of each type are there? Are the Fire options smaller than Fighter?
To answer these questions I have chosen this brief analysis. 

## Dataset

<br/>
The dataset is from Gurarako and you can get it [here.](https://www.kaggle.com/gurarako/basic-pandas-pokemon-dataset-part-1/data)
This dataset is complete, with all 807 Pokémon (7 generations). Here are included all evolutions as well as Mega Evolutions.
My first step was to prepare the data, using Python in Jupyter Notebook. I've imported Panda library to prepare the dataframes and for data visualization I've used Bokeh, Matplotlib and Seaborn libraries.

```python
#all imports
import pandas as pd #dataframes
from bokeh.io import show,output_notebook, output_file #bar chart
from bokeh.models import HoverTool, ColumnDataSource #bar chart
from bokeh.plotting import figure #bar chart
from bokeh.core.properties import value #bar chart
import matplotlib.pyplot as plt #heatmap
import seaborn as sns #heatmap
from math import pi

#make charts visibles in jupyter notebook
%matplotlib inline 
output_notebook() 
```

Once database imported, it was necessary to makes some changes due typos. The columns **Type 1** and **Type 2** had capitalize or lowercase strings. I've convert all letter to uppercase and sorted them to ascending.


```python
#using Pandas to import my csv file and create a data table
df = pd.read_csv('database/pokemonall.csv')
#make all rows uppercase
df['Type 1'] = df['Type 1'].str.upper()
df['Type 2'] = df['Type 2'].str.upper()
#sort by asceding
firstType = df['Type 1'].value_counts().sort_index(ascending=True)
secondType = df['Type 2'].value_counts().sort_index(ascending=True)
```

The result of my dataframe was:


|   | # |      Name| Type 1| Type 2| Total| HP| Attack| Defense| Sp. Atk| Sp. Def| Speed| Generation| Legendary|
|--:|--:|---------:|------:|------:|-----:| -:|------:|-------:|-------:|-------:|-----:|----------:|---------:|
| 0 | 1 | Bulbasaur|  GRASS| POISON|   318| 45|     49|      49|      65|      65|    45|          1|     False|
| 1 | 2 |   Ivysaur|  GRASS| POISON|   405| 60|     62|      63|      80|      80|    80|          1|     False|




## Charts: Bar

<br/>
The first chart tells us how many Pokémon are there for each type, here are considered all evolutions and also, if a Pokémon has two types, it is count twice. In this chart we can see the type amount per order, as an example we have Bulbasaur, it is primarily Grass and its secondary type is Poison. For this chart I've used Bokeh and we can mousehouver the chart to get the numbers for it order and type, check it out:

{% include bokeh_post02_types_count_by_order.html %}
<br/>
#### Script

```python
#creating a Bar chart with Bokeh
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

Looking to this chart, I must say, I've been quite impressed with tiny number of primary type of flying, it is the third largest group of Pokémon, we have a lot of them in the skies, but only 4 is primary! That makes me think: How many Flying Pokémon is pure(one-type)?
To know that I've plotted this second chart:

{% include bokeh_post02_one_type_pokemon_count.html %}
<br/>

To make it, I had to localize all blanks rows in column **Type 2** and put them in a *Series* of Pandas.

```python
indexNull = df[df['Type 2'].isnull()].index.tolist()
df2 = df.loc[indexNull,['Type 1']]
pureType = df2['Type 1'].value_counts().sort_index(ascending=True)
```

Then I've also used Bokeh to make this chart:


```python
#creating a Bar chart with Bokeh
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

## Charts: Heatmap

<br/>
Lastly, I've wanted to find out how is the cross-interaction between the primary and secondary types, so I've chosen a heatmap to vizualize this information.

Using the method `.crosstab` I've manipulated the dataframe to cross all Types 1 and 2, then I've created two totals with sum of columns and rows.
```python
#making new Dataframe crossing Type 1 and 2
crosstab_df = pd.crosstab(df['Type 1'], df['Type 2'])
#adding a total column
crosstab_df['Total'] = crosstab_df.sum(axis=1)
#adding a total row
finaltable = crosstab_df.append(pd.DataFrame(crosstab_df.sum(),
columns=['Total']).T, ignore_index=False)
#checking if worked
finaltable.tail()
```

With my data organized, I've used Matplotlib and Seaborn to plot the last chart. We can notice that the largest group of Pokémon with the same two-types are Flying with Normal. It is kind of expected, once Flying has a bigger number of secondary Type, Water has about half number Pokémon with just one type and Normal being the second biggest group of Type, with most of your Pokémon being primary.

```python
#creating heatmap
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

Awesome right? I hope you enjoyed it! 

You can check all analysis in my github [here.](https://github.com/shiguelita/whos_that_pokemon)
