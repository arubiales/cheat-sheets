# 1. Intro
Dash es una mezcla entre Flask y plotly, por lo que al saber los dos frameworks, me es bastante sencillo y familiar. Para crear una aplicación, hacemos lo mismo que con flask. Con Dash creamos la estructura web y con Plotly los gráficos

# 2. dash_core_components
Sirve para crear los componentes html más dificultosos, por ejemplo, el gráfico, el dropdown, upload, etc. Tiene componentes complejos, creados de forma sencilla

# 3. dash_html_components as html
Sirve para crear exactamente los mismo componentes que se generan en HTML, como por ejemplo un div, html, h1, h2, p, etc. A estos componentes mediante el atributo "style" se les puede hacer modificaciones CSS

# 4. Callbacks
Esta es una de las partes más importantes de dash, le pasamos un input (o multiples) y un output (o multiples). Se realiza mediante un decorador **el input siempre en una lista** con el id del componente y donde se encuentra la propiedad que se quiere modificar.

Estos Callbacks, permiten que la aplicación sea interactiva y que no se necesite cambiar de página web. Debajo va una función que transforma el input en el output deseado  

Además del Input y el Output ahí otra dependecia, que es el State, que permite que no se ejecuten los cambios, hasta que se clique un botón

# 5 Live update
Con la función interval de dash_core_components, podemos hacer updateos en vivo por intervalos del gráfico