---
layout: post
title: Who's that Pokémon?
date: 2018-07-19 12:00:00
comments: true
author: Talita Shiguemoto
---

# A data visualization analysis of Pokémon Types


For my first study I would like to start with something I really like: Pokémon!

If you are a Pokémon enthusiast as I am, you already thought: "I'd have a *choose your favorite type*-gym!". And in my case, it would be a Fighting or Fire type.

I was wordering, how many Pokémon of each type are there? Are the Fire options smaller than Fighter?

To answer these questions I have chosen this brief analysis. 

## Dataset


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

The first chart tells us how many Pokémon are there for each type, here are considered all evolutions and also, if a Pokémon has two types, it is count twice. In this chart we can see the type amount per order, as an example we have Bulbasaur, it is primarily Grass and yours secondary type is Poison. For this chart I've used Bokeh and we can mousehouver the chart to get the numbers for it order and type, check it out:



{% include bokeh_post02_types_count_by_order.html %}


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