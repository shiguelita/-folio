---
layout: post
title: Who's that Pokémon?
date: 2018-07-18 20:00:00
comments: true
author: Talita Shiguemoto
---

## Data visualization analysis of Pokémon Types


For my first study I would like to start with something I really like: Pokémon!
If you are a Pokémon enthusiast as I am, you already thought: "I'd have a *choose your favorite type*-gym!". And in my case, it would be a Fighting or Fire type.
I was wordering, how many Pokémon of each type are there? Are the Fire options smaller than Fighter?
To answer these questions I have chosen this brief analysis. 

# Dataset

The dataset is from Gurarako and you can get it <a href="https://www.kaggle.com/gurarako/basic-pandas-pokemon-dataset-part-1/data">here</a>
This dataset is complete, with all Pokémon until number 807 (7th Generation). Here are included all evolutions as well as Mega Evolutions.
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


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: center;
    }
</style>
<table border="0.5" class="dataframe">
  <thead>
    <tr style="text-align: center; font-size: 12px">
      <th></th>
      <th>#</th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Total</th>
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
      <th>Sp. Atk</th>
      <th>Sp. Def</th>
      <th>Speed</th>
      <th>Generation</th>
      <th>Legendary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Bulbasaur</td>
      <td>GRASS</td>
      <td>POISON</td>
      <td>318</td>
      <td>45</td>
      <td>49</td>
      <td>49</td>
      <td>65</td>
      <td>65</td>
      <td>45</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Ivysaur</td>
      <td>GRASS</td>
      <td>POISON</td>
      <td>405</td>
      <td>60</td>
      <td>62</td>
      <td>63</td>
      <td>80</td>
      <td>80</td>
      <td>60</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Venusaur</td>
      <td>GRASS</td>
      <td>POISON</td>
      <td>525</td>
      <td>80</td>
      <td>82</td>
      <td>83</td>
      <td>100</td>
      <td>100</td>
      <td>80</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>VenusaurMega Venusaur</td>
      <td>GRASS</td>
      <td>POISON</td>
      <td>625</td>
      <td>80</td>
      <td>100</td>
      <td>123</td>
      <td>122</td>
      <td>120</td>
      <td>80</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Charmander</td>
      <td>FIRE</td>
      <td>NaN</td>
      <td>309</td>
      <td>39</td>
      <td>52</td>
      <td>43</td>
      <td>60</td>
      <td>50</td>
      <td>65</td>
      <td>1</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>
<br/><br/>




# Charts: Bar

The first chart tells us how many Pokémon are there for each type, here are considered all evolutions and also, if a Pokémon has two types, it is count twice. In this chart we can see the type amount per order, as an example we have Bulbasaur, it is primarily Grass and yours secondary type is Poison. For this chart I've used Bokeh and we can mousehouver the chart to get the numbers for it order and type, check it out:



<div class="bk-root">
    <div class="bk-plotdiv" id="f1929a0d-a513-4b3b-b1f0-9cca2c797194"></div>
</div>


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
p.legend.location = "top_left"
p.legend.orientation = "vertical"
p.legend.spacing = 10
p.legend.padding = 20
p.legend.margin = 20

```