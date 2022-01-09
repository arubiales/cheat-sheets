# Pytest
Es el programa de testing más extendido y utilizado y con más frameworks alrededor

## Comandos
comandos que se le pueden poner a pytest, para agregar funcionalidad
* -v: Verbose, agrega información más bonita
* -rs: da informaicón agregada a los test que se han saltado es decir **la razón escrita en el marker**
* -m: si usamos el decorador **mark**, podemos llamar a una sola función y ejecutarle el test o un grupo de funciones que tenga el mismo marcador. Además uando comillas podemos introducir las ordenes lógicas, "and", "or", "not"
* -h: para ver todos los comandos
* --markers: te da los marcadores que hay implementados
* -s: para ver tus print()
* --html="nombre_archivo.html": nos da el resultado en html en un formato bonito (es necesario tener el paquete de html)
* --junitsml="nombre_archivo.xml": nos da el resultado en xml
* -n: número de hilos a usar. Sí usamos **nauto** se puede
* --ignore: ignora una carpeta


## pytest.ini
sirve para cambiar el comportamiento de pytest, por ejemplo que las funciones en vez de empezar por test_*

## Markers
es un decorador **@mark** nos permite marcar funciones, clases o grupos, para poder agruparlas o dividirlas a la hora de hacer test y no tener que hacer el test entero

## Fixtures
sirve para tener una función en todos los test que sea necesaria, sin necesidad de escribirla. El scope de está función (es decir su localización en memoria), siempre es la misma, es decir nunca podrás ejecutar dos a la vez. esto se puede cambiar dentro del decorador **@fixture** con el parámetro **scope** para ponerlo al nivel funcion, clase, todo el test, etc..

## Skiptest
Sirve para saltarse los test, pytest te avisa de los tests que te has saltado. para ello usamos un mark con: **mark.skip**,también te da la opción de dar una razón o de poner un if por ejemplo:
```
@mark.skip(reason='In development')
```

podemos ponerle una condición con **mark.skipif()**

## test fail
Es un marker que sirve para decirle a pytest que el contenido va a fallar, se usa un marker **mark.xfail**
```
@mark.xfail(reason="Env was not QA")
```

## Parametrization
sirve para pasar varios datos a un test y que los pruebe. para ello instalamos **pytest-xdist** los tres casos más útiles son:


## TOX
Es un Framework que nos ayuda a separar los test de la librería que hemos creado y nos permite testar en varios entornos

## Flask
en el caso de Flask viene con muchas funciones que ayudan a su testeo  

## Travis CI
Se trata de un testeo automático que cuando se sube el cógdigo a Github, nuestro código testea automáticamente el núevo código subido y si lo pasa se hace el commit. Tenemos que crearnos una cuenta

Para empezar hay que crear un YML llamado ".travis.yml" aquí estará la configuración de travis. Dentro de este archivo ponemos la configuración

```
language: python #el lenguaje que vamos a usar
python: #versión
  - "3.7"
  - "nightly"
install:
  - "pip install pytest" #instalamos el tester framework y los paquetes necesarios
  - "pip install -r requirements.txt"
script: python -m pytest tests/ #ponemos los que queremos lanzar
```

para poner las etiquetas de "build pass" en ell readme es necesario poner un linke a travis-ci en el readme


## Acceptance Testing
Es una forma de testear una aplicación web, haciendonos pasar por un usuario. Esto se hace con selenium 


# MI UTILIZACIÓN
yo lo que hago en la terminal es:

pytest -v --cov tests/ 

podemos usar -n4 por ejemplo o el número que sea, para hacerlo en multiprocessing.
Si ponemos nauto, lo decide automáaticamente

y pytest.ini lo configuro así

```
[pytest]
python_files = media_press/tests/test_*
python_classes = Tests*
python_functions = test_*
```

 python -m pytest -v --cov tests/