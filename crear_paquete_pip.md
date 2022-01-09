# Intro

* `__main__.py`: Es para poder hacer el archivo un ejecutable, si se hace, se puede utilizar `python -m nombre_modulo` para ejecutar el módulo.
* **Raiz de todo el paquete**: Es el primer `__init__.py` hay podemos tener la versión
* `setup.py`: es donde va toda la configuración de la instalación, puedes visitarlos para ver que hay

# Versiones
Hay manejadores de versiones que cambian automaticamente la versión para que no tengas tu que trastear por los ficheros. Uno de ellos es **Bumpversion**, se puede utilizar de la siguiente manera

`bump2version --current-version 1.0.0 minor setup.py reader/__init__.py`

Lo que hacemos es:
1. Llamar a `bump2version`
2. Decirle cual es nuestra versión actual `--current-version 1.0.0`
3. Decirle el tipo de cambio que queremos `minor` cambia el segundo número, `major` cambia el primero
4. Por último le indicamos donde se encuentran los archivos de la versión, es decir `setup.py` y `reader/__init__.py`

Hay una opción que te permite configurar `bump2version` para no tener que hacer todo esto, y solo hacer y el cambio que quieras `bump2version minor`

# Manifest
El archivo `MANIFEST.in` es un archivo que sive para que en tu instalación se incluyan los csv, txt etc. Que son necesario, para ello necesitas crear y configurar este archivo  

Por ejemplo, incluimos los `.txt` que se encuentran en una carpeta:
```
include reader/*.txt
```
si quiere que vaya recorriendo el paquete puedes hacer
```
include recursive skroute *.txt
```

Tambien se puede hacer que exclusa algo con `prunge`

Tenemos que tener en `setup.py` el apartado `include_package_data=True`

# Wheel construir distribuible
Ya tenemos todo ahora necesitamos configurar el archivo en un formato que a Pypy que guste, para ello usamos wheel. Situados en el `setup.py` raiz ejecutamos:

```
python setup.py sdist bdist_wheel
```

Esto nos creara nuevos ficheros:
* Build:
* Dist: donde se encuentra nuestro `.whl` y `.tar`

# Subir a pypy

Antes de subirlo debemos de checkear si todo está correcto, para ello u en el archivo `.tar` que hay dentro de la carpeta `dist`, ejecutamos `tar tzf <archivo>` y esto nos dará sin descomprimir el archivo lo que hay dentro.  

Después vemos si todo está correcto para pypy, para eso usamos `twine`. Ejecutando el siguiente comando nos chequea que ntodo este correcto:

```
python -m twine check dist/*
```

## Pypy test
Primero vamos a usar **pypi test** es igual que pypy solo que se usa para testear que todo esta bien antes de subirlo oficialmente a pypi. Para ello nos ponemos en la carpeta raíz y ejecutamos:

```
python -m twine upload --repository-url https://test.pypi.org/legacy/ dist/*
```

## Pypy real
Una vez vemos que va todo bien en el test, lo podemos subir al real. simplemente tenemos que quitar el `repository-url` de antes. Quedaría así:

```
python -m twine upload dist/*
```


# Hacer pip install en nuestro pc

Si hacemos `pip install <archivo.whl>` sobre nuestro archivo **dentro de dist** `.whl` podremos probar una instalación del mismo en local


# Configuración para archivos Cython

1. Primero de todo cythonizamos los archivos cython, así:
```
extensions = cythonize(Extension(
          "core_2.algorithms.algo1._base_algo._base_algo",
          sources=["core_2/algorithms/algo1/_base_algo/_base_algo.pyx"]
          ))
```

2. Despues añadimos `ext_modules = extensions` para que instale los archivos cython
3. Seguimos el proceso hasta antes de subir el archivo a Pypi
4. [Esta pregunta](https://stackoverflow.com/questions/47042483/how-to-build-and-distribute-a-python-cython-package-that-depends-on-third-party) es la solución. Si no tenemos auditwheel instalado y los paquetes necesario los instalamos según la respuesta
5. Siguiendo la respuesta hacemos `auditwheel repair <archivo.whl>` Esto nos creará una carpeta con un nuevo `.whl`
6. Borramos el antiguo `.whl` de setuptool y lo sustituimos por el de `auditwheel`. Borramos la carpeta de **auditwheel** para que quede todo como al inicio, pero con el nuevo `.whl` sustituido
7. Hacemos `python -m twine upload dist/*` al igual que siempre


# Lo que hacemos en skroute
1. Lanzamos los test una vez el paquete esta instalado
2. Los lanzamos desde fuera de la carpeta
3. Para los test es muy recomendable venv y usar pip para los requirements.txt
