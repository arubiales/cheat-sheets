# 1. Intro
Las principales sublibrerías de plotly son plotly.express, plotly.graph_objects y plotly.offline

# 2. Gráficos
Los principales gráficos son:
* Scatterplot
* Linechart
* Barchart
* Buble
* Boxplot
* Histogram
* Distplot
* Heatmaps

Aunque hay muchísimos más que puedes ver en su documentación

# 3. Ejecución

1- Creamos un Graph Object que tienen que ir dentro de una lista, aquí se especifican los datos el gráfico y sus propiedades
2- Creamos un Layout donde definimos distintos elementos de estilo del anterior gráfico. Por ejmemplo, título, nombre de las variables, etc.
3- Creamos una figura donde juntamos el Graph Object y el Layout
4- Por último ploteamos el objeto figura, con plotly offline y con el atríbuto "filename" podemos fijar donde queremos guardarlo  ç

**Como veremos Dash al ser invención de plotly es bastante parecido**