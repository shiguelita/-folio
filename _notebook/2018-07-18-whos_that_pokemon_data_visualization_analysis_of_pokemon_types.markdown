---
layout: post
title: Who's that Pokémon?
date: 2018-07-18 20:00:00
comments: true
author: Talita Shiguemoto
---

##  Data visualization analysis of Pokémon Types


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
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: center;">
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





# Charts: Bar

The first chart tells us how many Pokémon are there for each type, here are considered all evolutions and also, if a Pokémon has two types, it is count twice. In this chart we can see the type amount per order, as an example we have Bulbasaur, it is primarily Grass and yours secondary type is Poison. For this chart I've used Bokeh and we can mousehouver the chart to get the numbers for it order and type, check it out:


       

       <div class="bk-root">
            <div class="bk-plotdiv" id="bc5ffe50-0adf-4e49-a67e-118d624eef4a"></div>
        </div>
        
        <script type="application/json" id="cb45f9e4-6254-41bf-ac80-91e7826e3aa8">
          {"7ad0c208-82dd-4cf6-8d7d-e32355043d48":{"roots":{"references":[{"attributes":{"formatter":{"id":"02ee62cb-3ce8-40bf-9824-24920813f047","type":"BasicTickFormatter"},"minor_tick_line_color":{"value":null},"plot":{"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},"ticker":{"id":"40462d13-dd66-4a47-a3f8-b98dacc680b9","type":"BasicTicker"}},"id":"0a06e35a-104f-425d-8824-185d52b13fc9","type":"LinearAxis"},{"attributes":{"callback":null,"factors":["BUG","DARK","DRAGON","ELECTRIC","FAIRY","FIGHTING","FIRE","FLYING","GHOST","GRASS","GROUND","ICE","NORMAL","POISON","PSYCHIC","ROCK","STEEL","WATER"],"range_padding":0.1},"id":"fb8f0632-957c-4b18-af8f-323c6d43bdd5","type":"FactorRange"},{"attributes":{},"id":"710f300f-a7f0-4fdd-a8d6-56e317046433","type":"CategoricalScale"},{"attributes":{"label":{"value":"Primary"},"renderers":[{"id":"010ae4ee-934c-427c-86ee-44b050edc435","type":"GlyphRenderer"}]},"id":"b930e01d-edbf-4437-90a3-8953bc1661a2","type":"LegendItem"},{"attributes":{},"id":"4f78324f-2d44-4575-ba8b-4cba3c7105ea","type":"LinearScale"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_scroll":"auto","active_tap":"auto","tools":[{"id":"77b2490c-7480-4fa6-b5a7-ab3debe69850","type":"HoverTool"}]},"id":"b2ac829e-2137-4033-8aa8-7b9e35415914","type":"Toolbar"},{"attributes":{"formatter":{"id":"19c30ba2-9933-4508-80ed-57dacc3c749c","type":"CategoricalTickFormatter"},"major_label_orientation":1.5707963267948966,"minor_tick_line_color":{"value":null},"plot":{"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},"ticker":{"id":"7ce0c5ce-cbe1-4fa5-ab1c-be45c59b4ffe","type":"CategoricalTicker"}},"id":"1df1827e-0bea-422c-95ab-75710005c8a2","type":"CategoricalAxis"},{"attributes":{"callback":null,"start":0},"id":"a5ff430f-3440-4f78-b73e-e91e09b4bd3e","type":"DataRange1d"},{"attributes":{},"id":"7ce0c5ce-cbe1-4fa5-ab1c-be45c59b4ffe","type":"CategoricalTicker"},{"attributes":{},"id":"8400b0e0-333b-4071-a5e9-2b9b9c4a9a58","type":"CategoricalScale"},{"attributes":{"fields":[]},"id":"75ff9a80-a884-46fc-a157-53b2d51ac80a","type":"Stack"},{"attributes":{},"id":"2c386547-3440-49cf-97df-17a6b68ab3c1","type":"LinearScale"},{"attributes":{"items":[{"id":"7fff09a3-b347-403e-a39d-372798fa4216","type":"LegendItem"},{"id":"1b3ae65a-8a2c-483e-b059-f7b5f01d0de6","type":"LegendItem"}],"location":"top_left","margin":20,"padding":20,"plot":{"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},"spacing":10},"id":"48651050-b83f-4321-8437-a826fe2a84ce","type":"Legend"},{"attributes":{"grid_line_color":{"value":null},"plot":{"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},"ticker":{"id":"7ce0c5ce-cbe1-4fa5-ab1c-be45c59b4ffe","type":"CategoricalTicker"}},"id":"0ecc5279-45c4-44ec-bec5-63db26ede8a3","type":"Grid"},{"attributes":{"formatter":{"id":"25d5d0b5-d0e1-4e0d-9ae6-493e15162b05","type":"BasicTickFormatter"},"minor_tick_line_color":{"value":null},"plot":{"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},"ticker":{"id":"03a69b73-25df-4a02-aa2b-a67cd77e6e98","type":"BasicTicker"}},"id":"9591585b-2b79-40d3-87e9-315e87a6aac6","type":"LinearAxis"},{"attributes":{},"id":"40462d13-dd66-4a47-a3f8-b98dacc680b9","type":"BasicTicker"},{"attributes":{"formatter":{"id":"117baf84-d36b-43ca-b437-bdc72bedd476","type":"CategoricalTickFormatter"},"major_label_orientation":1.5707963267948966,"minor_tick_line_color":{"value":null},"plot":{"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},"ticker":{"id":"8ca002d9-36f8-49c9-9f34-1a5775eaa8e7","type":"CategoricalTicker"}},"id":"be836bb2-7211-4876-9698-de11c4f7bd9b","type":"CategoricalAxis"},{"attributes":{},"id":"8ca002d9-36f8-49c9-9f34-1a5775eaa8e7","type":"CategoricalTicker"},{"attributes":{"bottom":{"expr":{"id":"75ff9a80-a884-46fc-a157-53b2d51ac80a","type":"Stack"}},"fill_color":{"value":"#3182bd"},"line_color":{"value":"#3182bd"},"top":{"expr":{"id":"2985d6fe-8016-4fa8-85db-c6dfde82219c","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"09578951-1a8a-4f6a-96a2-fbcb2719beca","type":"VBar"},{"attributes":{"grid_line_color":{"value":null},"plot":{"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},"ticker":{"id":"8ca002d9-36f8-49c9-9f34-1a5775eaa8e7","type":"CategoricalTicker"}},"id":"42adfa2f-a89c-455c-af65-bfb11061fb23","type":"Grid"},{"attributes":{"fields":["Primary"]},"id":"2985d6fe-8016-4fa8-85db-c6dfde82219c","type":"Stack"},{"attributes":{"fields":[]},"id":"443dae99-ee00-4a25-9ea1-5c86109baa99","type":"Stack"},{"attributes":{"dimension":1,"plot":{"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},"ticker":{"id":"40462d13-dd66-4a47-a3f8-b98dacc680b9","type":"BasicTicker"}},"id":"bd90962d-5207-4ec3-b4ab-655975c077f1","type":"Grid"},{"attributes":{},"id":"03a69b73-25df-4a02-aa2b-a67cd77e6e98","type":"BasicTicker"},{"attributes":{"fields":["Primary"]},"id":"4b69fb96-c013-4691-812d-c35bd48b02d1","type":"Stack"},{"attributes":{"dimension":1,"plot":{"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},"ticker":{"id":"03a69b73-25df-4a02-aa2b-a67cd77e6e98","type":"BasicTicker"}},"id":"11715ab9-b4c1-46e7-93ee-27eb71aed2d0","type":"Grid"},{"attributes":{"fields":["Primary","Secondary"]},"id":"d407d8ac-07d2-4741-a04c-fc46fe4856ca","type":"Stack"},{"attributes":{"fields":["Primary"]},"id":"a8b12d7a-bc04-419c-9575-315a200d48c2","type":"Stack"},{"attributes":{"label":{"value":"Primary"},"renderers":[{"id":"6721e08c-4c51-40f9-b97e-eb1a2eab850a","type":"GlyphRenderer"}]},"id":"7fff09a3-b347-403e-a39d-372798fa4216","type":"LegendItem"},{"attributes":{"bottom":{"expr":{"id":"4b69fb96-c013-4691-812d-c35bd48b02d1","type":"Stack"}},"fill_color":{"value":"#9ecae1"},"line_color":{"value":"#9ecae1"},"top":{"expr":{"id":"d407d8ac-07d2-4741-a04c-fc46fe4856ca","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"7b37a273-f0c9-4b07-89ae-c28fe566500f","type":"VBar"},{"attributes":{},"id":"117baf84-d36b-43ca-b437-bdc72bedd476","type":"CategoricalTickFormatter"},{"attributes":{},"id":"19c30ba2-9933-4508-80ed-57dacc3c749c","type":"CategoricalTickFormatter"},{"attributes":{"fields":["Primary"]},"id":"8cdd505b-1c3a-4cbf-bba3-ed6b4b896cd6","type":"Stack"},{"attributes":{"fields":["Primary","Secondary"]},"id":"cfff5034-7f0e-4917-8106-137a360a169b","type":"Stack"},{"attributes":{},"id":"02ee62cb-3ce8-40bf-9824-24920813f047","type":"BasicTickFormatter"},{"attributes":{"bottom":{"expr":{"id":"4b69fb96-c013-4691-812d-c35bd48b02d1","type":"Stack"}},"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"top":{"expr":{"id":"d407d8ac-07d2-4741-a04c-fc46fe4856ca","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"df8383dc-d860-405d-8933-4a9945299e20","type":"VBar"},{"attributes":{"bottom":{"expr":{"id":"443dae99-ee00-4a25-9ea1-5c86109baa99","type":"Stack"}},"fill_color":{"value":"#3182bd"},"line_color":{"value":"#3182bd"},"top":{"expr":{"id":"8cdd505b-1c3a-4cbf-bba3-ed6b4b896cd6","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"a82f16ab-d6b0-418e-bdd0-ec266789d652","type":"VBar"},{"attributes":{"data_source":{"id":"8d0df258-b158-4c78-9722-4ad75afbd52d","type":"ColumnDataSource"},"glyph":{"id":"09578951-1a8a-4f6a-96a2-fbcb2719beca","type":"VBar"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"4a47387f-c5b8-43f8-98d5-1b4257eb0c11","type":"VBar"},"selection_glyph":null,"view":{"id":"ccd56660-8237-4507-a8b1-2f9efee726a6","type":"CDSView"}},"id":"6721e08c-4c51-40f9-b97e-eb1a2eab850a","type":"GlyphRenderer"},{"attributes":{"bottom":{"expr":{"id":"443dae99-ee00-4a25-9ea1-5c86109baa99","type":"Stack"}},"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"top":{"expr":{"id":"8cdd505b-1c3a-4cbf-bba3-ed6b4b896cd6","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"8fb4483e-87f3-4d6b-8692-8a22fe53ec03","type":"VBar"},{"attributes":{"bottom":{"expr":{"id":"75ff9a80-a884-46fc-a157-53b2d51ac80a","type":"Stack"}},"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"top":{"expr":{"id":"2985d6fe-8016-4fa8-85db-c6dfde82219c","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"4a47387f-c5b8-43f8-98d5-1b4257eb0c11","type":"VBar"},{"attributes":{"source":{"id":"8d0df258-b158-4c78-9722-4ad75afbd52d","type":"ColumnDataSource"}},"id":"ccd56660-8237-4507-a8b1-2f9efee726a6","type":"CDSView"},{"attributes":{"bottom":{"expr":{"id":"a8b12d7a-bc04-419c-9575-315a200d48c2","type":"Stack"}},"fill_color":{"value":"#9ecae1"},"line_color":{"value":"#9ecae1"},"top":{"expr":{"id":"cfff5034-7f0e-4917-8106-137a360a169b","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"0a79a319-f5dd-4676-a64f-648eea508cdf","type":"VBar"},{"attributes":{"below":[{"id":"1df1827e-0bea-422c-95ab-75710005c8a2","type":"CategoricalAxis"}],"left":[{"id":"0a06e35a-104f-425d-8824-185d52b13fc9","type":"LinearAxis"}],"outline_line_color":{"value":null},"plot_height":400,"renderers":[{"id":"1df1827e-0bea-422c-95ab-75710005c8a2","type":"CategoricalAxis"},{"id":"0ecc5279-45c4-44ec-bec5-63db26ede8a3","type":"Grid"},{"id":"0a06e35a-104f-425d-8824-185d52b13fc9","type":"LinearAxis"},{"id":"bd90962d-5207-4ec3-b4ab-655975c077f1","type":"Grid"},{"id":"48651050-b83f-4321-8437-a826fe2a84ce","type":"Legend"},{"id":"6721e08c-4c51-40f9-b97e-eb1a2eab850a","type":"GlyphRenderer"},{"id":"d59c1291-7e1c-49ee-bf5a-8123efbe671c","type":"GlyphRenderer"}],"title":{"id":"55254096-9d39-4ac6-b1e1-fa8595fa4d0b","type":"Title"},"toolbar":{"id":"3c22fe98-3a41-49ba-81e4-35535c35e48a","type":"Toolbar"},"toolbar_location":null,"x_range":{"id":"75d26fe8-23b6-43b5-baac-702fc8900396","type":"FactorRange"},"x_scale":{"id":"710f300f-a7f0-4fdd-a8d6-56e317046433","type":"CategoricalScale"},"y_range":{"id":"dacac524-2442-4a91-8b2b-fdeb44c24bca","type":"DataRange1d"},"y_scale":{"id":"4f78324f-2d44-4575-ba8b-4cba3c7105ea","type":"LinearScale"}},"id":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7","subtype":"Figure","type":"Plot"},{"attributes":{},"id":"751e12de-b571-4664-8928-dbc8f88c4892","type":"UnionRenderers"},{"attributes":{},"id":"763d835f-b38f-43f6-93d8-d1be9a08856f","type":"UnionRenderers"},{"attributes":{},"id":"cec8c4b4-6564-4093-bb51-542cf532b5ad","type":"Selection"},{"attributes":{},"id":"25d5d0b5-d0e1-4e0d-9ae6-493e15162b05","type":"BasicTickFormatter"},{"attributes":{"source":{"id":"ef43be28-3e06-4992-ad73-c663c7f9b7c1","type":"ColumnDataSource"}},"id":"ac67231a-5ec8-4511-a993-173d80b42580","type":"CDSView"},{"attributes":{"data_source":{"id":"ef43be28-3e06-4992-ad73-c663c7f9b7c1","type":"ColumnDataSource"},"glyph":{"id":"a82f16ab-d6b0-418e-bdd0-ec266789d652","type":"VBar"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"8fb4483e-87f3-4d6b-8692-8a22fe53ec03","type":"VBar"},"selection_glyph":null,"view":{"id":"eaf1f49b-d0d9-46b1-b5e1-fba66cbb4500","type":"CDSView"}},"id":"010ae4ee-934c-427c-86ee-44b050edc435","type":"GlyphRenderer"},{"attributes":{"active_drag":"auto","active_inspect":"auto","active_scroll":"auto","active_tap":"auto","tools":[{"id":"5ff49775-5445-4197-b730-5a780139becf","type":"HoverTool"}]},"id":"3c22fe98-3a41-49ba-81e4-35535c35e48a","type":"Toolbar"},{"attributes":{"data_source":{"id":"8d0df258-b158-4c78-9722-4ad75afbd52d","type":"ColumnDataSource"},"glyph":{"id":"7b37a273-f0c9-4b07-89ae-c28fe566500f","type":"VBar"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"df8383dc-d860-405d-8933-4a9945299e20","type":"VBar"},"selection_glyph":null,"view":{"id":"20135c57-88ed-4fea-99af-145ce1514718","type":"CDSView"}},"id":"d59c1291-7e1c-49ee-bf5a-8123efbe671c","type":"GlyphRenderer"},{"attributes":{"source":{"id":"ef43be28-3e06-4992-ad73-c663c7f9b7c1","type":"ColumnDataSource"}},"id":"eaf1f49b-d0d9-46b1-b5e1-fba66cbb4500","type":"CDSView"},{"attributes":{"callback":null,"renderers":"auto","tooltips":[["Primary","@Primary"],["Secondary","@Secondary"],["Type","@Types"]]},"id":"5ff49775-5445-4197-b730-5a780139becf","type":"HoverTool"},{"attributes":{"label":{"value":"Secondary"},"renderers":[{"id":"d59c1291-7e1c-49ee-bf5a-8123efbe671c","type":"GlyphRenderer"}]},"id":"1b3ae65a-8a2c-483e-b059-f7b5f01d0de6","type":"LegendItem"},{"attributes":{"items":[{"id":"b930e01d-edbf-4437-90a3-8953bc1661a2","type":"LegendItem"},{"id":"9838fef9-5fb1-4de2-aa90-5a8239d0b66f","type":"LegendItem"}],"location":"top_left","margin":20,"padding":20,"plot":{"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},"spacing":10},"id":"a4b0ec05-508e-4721-92a9-4d6d50bf0a5b","type":"Legend"},{"attributes":{"callback":null,"start":0},"id":"dacac524-2442-4a91-8b2b-fdeb44c24bca","type":"DataRange1d"},{"attributes":{"source":{"id":"8d0df258-b158-4c78-9722-4ad75afbd52d","type":"ColumnDataSource"}},"id":"20135c57-88ed-4fea-99af-145ce1514718","type":"CDSView"},{"attributes":{"callback":null,"factors":["BUG","DARK","DRAGON","ELECTRIC","FAIRY","FIGHTING","FIRE","FLYING","GHOST","GRASS","GROUND","ICE","NORMAL","POISON","PSYCHIC","ROCK","STEEL","WATER"],"range_padding":0.1},"id":"75d26fe8-23b6-43b5-baac-702fc8900396","type":"FactorRange"},{"attributes":{"callback":null,"renderers":"auto","tooltips":[["Primary","@Primary"],["Secondary","@Secondary"],["Type","@Types"]]},"id":"77b2490c-7480-4fa6-b5a7-ab3debe69850","type":"HoverTool"},{"attributes":{"data_source":{"id":"ef43be28-3e06-4992-ad73-c663c7f9b7c1","type":"ColumnDataSource"},"glyph":{"id":"0a79a319-f5dd-4676-a64f-648eea508cdf","type":"VBar"},"hover_glyph":null,"muted_glyph":null,"nonselection_glyph":{"id":"9a9e4ee7-cb3f-48b0-a721-25d0436c37b4","type":"VBar"},"selection_glyph":null,"view":{"id":"ac67231a-5ec8-4511-a993-173d80b42580","type":"CDSView"}},"id":"2e79674b-a5d6-4ca8-9090-9b223fb447c7","type":"GlyphRenderer"},{"attributes":{"plot":null,"text":"Types Counts by Order"},"id":"55254096-9d39-4ac6-b1e1-fa8595fa4d0b","type":"Title"},{"attributes":{"below":[{"id":"be836bb2-7211-4876-9698-de11c4f7bd9b","type":"CategoricalAxis"}],"left":[{"id":"9591585b-2b79-40d3-87e9-315e87a6aac6","type":"LinearAxis"}],"outline_line_color":{"value":null},"plot_height":400,"renderers":[{"id":"be836bb2-7211-4876-9698-de11c4f7bd9b","type":"CategoricalAxis"},{"id":"42adfa2f-a89c-455c-af65-bfb11061fb23","type":"Grid"},{"id":"9591585b-2b79-40d3-87e9-315e87a6aac6","type":"LinearAxis"},{"id":"11715ab9-b4c1-46e7-93ee-27eb71aed2d0","type":"Grid"},{"id":"a4b0ec05-508e-4721-92a9-4d6d50bf0a5b","type":"Legend"},{"id":"010ae4ee-934c-427c-86ee-44b050edc435","type":"GlyphRenderer"},{"id":"2e79674b-a5d6-4ca8-9090-9b223fb447c7","type":"GlyphRenderer"}],"title":{"id":"bb1d7341-8850-4299-bf9c-b1a3263b0030","type":"Title"},"toolbar":{"id":"b2ac829e-2137-4033-8aa8-7b9e35415914","type":"Toolbar"},"toolbar_location":null,"x_range":{"id":"fb8f0632-957c-4b18-af8f-323c6d43bdd5","type":"FactorRange"},"x_scale":{"id":"8400b0e0-333b-4071-a5e9-2b9b9c4a9a58","type":"CategoricalScale"},"y_range":{"id":"a5ff430f-3440-4f78-b73e-e91e09b4bd3e","type":"DataRange1d"},"y_scale":{"id":"2c386547-3440-49cf-97df-17a6b68ab3c1","type":"LinearScale"}},"id":"0ca63b57-a3bd-4194-af91-7241852531b7","subtype":"Figure","type":"Plot"},{"attributes":{"bottom":{"expr":{"id":"a8b12d7a-bc04-419c-9575-315a200d48c2","type":"Stack"}},"fill_alpha":{"value":0.1},"fill_color":{"value":"#1f77b4"},"line_alpha":{"value":0.1},"line_color":{"value":"#1f77b4"},"top":{"expr":{"id":"cfff5034-7f0e-4917-8106-137a360a169b","type":"Stack"}},"width":{"value":0.9},"x":{"field":"Types"}},"id":"9a9e4ee7-cb3f-48b0-a721-25d0436c37b4","type":"VBar"},{"attributes":{"callback":null,"data":{"Primary":[78,32,35,48,18,31,58,4,36,82,34,24,110,34,63,49,29,121],"Secondary":[5,21,22,8,34,32,14,105,18,26,37,15,4,35,35,14,26,18],"Types":["BUG","DARK","DRAGON","ELECTRIC","FAIRY","FIGHTING","FIRE","FLYING","GHOST","GRASS","GROUND","ICE","NORMAL","POISON","PSYCHIC","ROCK","STEEL","WATER"]},"selected":{"id":"cec8c4b4-6564-4093-bb51-542cf532b5ad","type":"Selection"},"selection_policy":{"id":"751e12de-b571-4664-8928-dbc8f88c4892","type":"UnionRenderers"}},"id":"8d0df258-b158-4c78-9722-4ad75afbd52d","type":"ColumnDataSource"},{"attributes":{"callback":null,"data":{"Primary":[78,32,35,48,18,31,58,4,36,82,34,24,110,34,63,49,29,121],"Secondary":[5,21,22,8,34,32,14,105,18,26,37,15,4,35,35,14,26,18],"Types":["BUG","DARK","DRAGON","ELECTRIC","FAIRY","FIGHTING","FIRE","FLYING","GHOST","GRASS","GROUND","ICE","NORMAL","POISON","PSYCHIC","ROCK","STEEL","WATER"]},"selected":{"id":"0b7c7447-78b4-4104-9701-3857d3591af3","type":"Selection"},"selection_policy":{"id":"763d835f-b38f-43f6-93d8-d1be9a08856f","type":"UnionRenderers"}},"id":"ef43be28-3e06-4992-ad73-c663c7f9b7c1","type":"ColumnDataSource"},{"attributes":{"label":{"value":"Secondary"},"renderers":[{"id":"2e79674b-a5d6-4ca8-9090-9b223fb447c7","type":"GlyphRenderer"}]},"id":"9838fef9-5fb1-4de2-aa90-5a8239d0b66f","type":"LegendItem"},{"attributes":{},"id":"0b7c7447-78b4-4104-9701-3857d3591af3","type":"Selection"},{"attributes":{"plot":null,"text":"Types Counts by Order"},"id":"bb1d7341-8850-4299-bf9c-b1a3263b0030","type":"Title"}],"root_ids":["0ca63b57-a3bd-4194-af91-7241852531b7","9655b7ea-507f-4a5e-9d9d-98ba64578cd7"]},"title":"Bokeh Application","version":"0.12.16"}}
        </script>
        <script type="text/javascript">
          (function() {
            var fn = function() {
              Bokeh.safely(function() {
                (function(root) {
                  function embed_document(root) {
                    
                  var docs_json = document.getElementById('cb45f9e4-6254-41bf-ac80-91e7826e3aa8').textContent;
                  var render_items = [{"docid":"7ad0c208-82dd-4cf6-8d7d-e32355043d48","elementid":"bc5ffe50-0adf-4e49-a67e-118d624eef4a","modelid":"9655b7ea-507f-4a5e-9d9d-98ba64578cd7"}];
                  root.Bokeh.embed.embed_items(docs_json, render_items);
                
                  }
                  if (root.Bokeh !== undefined) {
                    embed_document(root);
                  } else {
                    var attempts = 0;
                    var timer = setInterval(function(root) {
                      if (root.Bokeh !== undefined) {
                        embed_document(root);
                        clearInterval(timer);
                      }
                      attempts++;
                      if (attempts > 100) {
                        console.log("Bokeh: ERROR: Unable to run BokehJS code because BokehJS library is missing")
                        clearInterval(timer);
                      }
                    }, 10, root)
                  }
                })(window);
              });
            };
            if (document.readyState != "loading") fn();
            else document.addEventListener("DOMContentLoaded", fn);
          })();
        </script>





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