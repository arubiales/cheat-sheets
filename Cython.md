## Utilización en Jupyter Notebooks

Es sencillo tan solo tenemos que llamar en la celda de Jupyter la celda mágica **%%Cython**

```
%%cython

def factorial_cyData(n):
    cdef int fact = 1
    cdef int x
    for x in range(2, n+1):
        fact = fact* x
    return fact
```


# Utilización en Script

creamos un archivo ```.pyx``` y un archivo ```setup.py``` para crear el paquete. Ejemplo:

## Archivo .pyx
```
def factorial_cyData(n):
    cdef int fact = 1
    cdef int x
    for x in range(2, n+1):
        fact = fact* x
    return fact
```

## archivo setup.py

```
from setuptools import setup
from Cython.Build import cythonize

setup(ext_modules = cythonize("fact.pyx", annotate=True)) #esto nos da un html, si el código es blanco significa que no interactua con python, si es amarillo significa que usa python y que por tanto es más lento

```

**Una vez hecho esto, llamamo a setup.py para compilar el archivo .pys** para ello usamos el siguiente comando:
```
python setup.py build_ext --inplace
```

# Cython Data Types
Para definirlos usamos la palabra clave ```cdef```

## Enteros (integers):
* long
* int
* unsigned short
* unsigned long
etc.

## Float
* float
* double

## String 
* str

## Complex
* complex

## Ejemplos
```
cdef int x = 0
cdef long y = 1
cdef unsigned long z = 100
cdef long long abc = 11111111111111
```

```
cdef:
    int x2=0
    long y2=1
```


## Funciones
en Cython tenemos tres tipos de funciones:
* def: es como crear la función en python
* cdef: es crear la función en C, tienen las siguientes normas:
  * No pueden ser definidas dentro de otras funciones
  * Solo soportan los tipos de C
  * No son accesibles por el Scope de Python
* cpdef: es un hibrido entre Python y C:
  * Es recomendable usarlas cunado utilizas funciones de Cython dentro de Python


# Sintaxis

```inline```: cuando definimos sintaxis de python inline, es decir list_comprenhension, sentencias if en una linea, etc. Además se usa en funciones que se usan dentro de otras funciones, para evitar llamadas, y que formen parte de la misma función, aumetando drásticamente la velocidad


# Usando cosas de C

## Usamos el vector 

```
from libcpp.vector cimport vector

def primes(unsigned int nb_primes):
    cdef int n, i
    cdef vector[int] p
    p.reserve(nb_primes)  # allocate memory for 'nb_primes' elements.

    n = 2
    while p.size() < nb_primes:  # size() for vectors is similar to len()
        for i in p:
            if n % i == 0:
                break
        else:
            p.push_back(n)  # push_back is similar to append()
        n += 1

    # Vectors are automatically converted to Python
    # lists when converted to Python objects.
    return p
```


## Usamos funciones que están en la stdlib

* atoi

```
from libc.stdlib cimport atoi

cdef parse_charptr_to_py_int(char* s):
    assert s is not NULL, "byte string value is NULL"
    return atoi(s)  # note: atoi() has no error detection!
```


## Exportando módulos de C

```
cdef extern from "math.h":
    double sin(double x)
```

Creamos la extrauuctura queue en cython
```
# queue.pyx

cimport cqueue

cdef class Queue:
    """A queue class for C integer values.

    >>> q = Queue()
    >>> q.append(5)
    >>> q.peek()
    5
    >>> q.pop()
    5
    """
    cdef cqueue.Queue* _c_queue
    def __cinit__(self):
        self._c_queue = cqueue.queue_new()
        if self._c_queue is NULL: #Necesitamos comprobar si ha habido memoria para crear la queue
            raise MemoryError()

    def __dealloc__(self):
        if self._c_queue is not NULL: #Borramos la memoria
            cqueue.queue_free(self._c_queue)

    cpdef append(self, int value):
        if not cqueue.queue_push_tail(self._c_queue,
                                      <void*> <Py_ssize_t> value):
            raise MemoryError()

    # The `cpdef` feature is obviously not available for the original "extend()"
    # method, as the method signature is incompatible with Python argumentºº
    # types (Python does not have pointers).  However, we can rename
    # the C-ish "extend()" method to e.g. "extend_ints()", and write
    # a new "extend()" method that provides a suitable Python interface by
    # accepting an arbitrary Python iterable.
    cpdef extend(self, values):
        for value in values:
            self.append(value)

    cdef extend_ints(self, int* values, size_t count):
        cdef int value
        for value in values[:count]:  # Slicing pointer to limit the iteration boundaries.
            self.append(value)

    cpdef int peek(self) except? -1:
        cdef int value = <Py_ssize_t> cqueue.queue_peek_head(self._c_queue)

        if value == 0:
            # this may mean that the queue is empty,
            # or that it happens to contain a 0 value
            if cqueue.queue_is_empty(self._c_queue):
                raise IndexError("Queue is empty")
        return value

    cpdef int pop(self) except? -1:
        if cqueue.queue_is_empty(self._c_queue):
            raise IndexError("Queue is empty")
        return <Py_ssize_t> cqueue.queue_pop_head(self._c_queue)

    def __bool__(self):
        return not cqueue.queue_is_empty(self._c_queue)
```

## Cython al igual que C permite declaración de funciones sin parametros

```
cdef extern from "string.h":
    char* strstr(const char*, const char*)
```



# Clases
Al ser una mezcla entre python y C es un mix:
* Los atributos deben ser declarados al principio al momento de compilar, y podemos usar ```readonly``` para podemr consultarlo o ``
* Solo son accesibles desde cython
* el constructor ```__cinit__```, sirve para construir cosas de C, como arrays, vectores, etc.
* Se pueden poner los métodos como CDEF pero tienen que ir completamente en C y solo llevar el self.
* Si añadimos inline a los métodos cdef puede que logremos una pequeña mejora de velocidad
* Los métodos especiales, siempre usan **def**  como por ejemplo ```__init__```


* **Es necesario crear un .h definiendo a las funciones junto con el .cpp**ç

```
#ifndef RANDOM_H
#define RANDOM_H
using namespace std;

float _randuniform();

int _randint(int min_val, int max_val);

#endif
```

** Aquí el .cpp de ejemplo**

```

#include <Python.h>
#include <iostream>
#include <functional>
#include <iterator>
#include <algorithm>
#include <random>
#include <numeric>
#include "random.h"

using namespace std;

float _randuniform(){
    std::random_device rand_dev;
    std::mt19937 generator(rand_dev());
    std::uniform_real_distribution<float>  distr(0, 1);

    return distr(generator);
}

int _randint(int min_val, int max_val){
    std::random_device rd;     // only used once to initialise (seed) engine
    std::mt19937 rng(rd());    // random-number engine used (Mersenne-Twister in this case)
    std::uniform_int_distribution<int> uni(min_val,max_val); // guaranteed unbiased

    int random_integer = uni(rng);
    return random_integer;
}

```

**El setup.py**

```
from setuptools import Extension, setup
from Cython.Build import cythonize

setup(ext_modules = cythonize(Extension(
           "Cython_GA_Builder",                                # the extension name
           sources=["Cython_GA_Builder.pyx"], # the Cython source and
                                                  # additional C++ source files
           language="c++",                        # generate and compile C++ code
      )))

```


**Y  por último importariamos en el .pys así**

```
cdef extern from "random.h":
    cdef float _randuniform()
    cdef int _randint(int min_val, int max_val)
```

```
from sin_of_square cimport Function

cdef class WaveFunction(Function):

    # Not available in Python-space:
    cdef double offset

    # Available in Python-space:
    cdef public double freq

    # Available in Python-space, but only for reading:
    cdef readonly double scale

    # Available in Python-space:
    @property
    def period(self):
        return 1.0 / self.freq

    @period.setter
    def period(self, value):
        self.freq = 1.0 / value

```

# .pxd Files
Al igual que **C** Cython usa .pxd que son los **headers** en C. Estos archivos .pxd son importados a CYthon.  

Cython al igual que python, puedes crear archivos ```__init__.py``` solo que serán ```__init__.pxd```


# Profiling
Esto pertenece a Python. Es la forma de calcular el tiempo que se tarda en realizar un programa y va a analizando linea de código por linea. Las columnas más impotartes para su resultado son:
* **totime:**  total time spend, el tiempo total gastado en la función sin las funciones llamadas por esta función
* **cumtime:** tiempo total gastado también contando con esta función contando también el resto de funciones que llama esta función
  

Profiling con Cython permite crear un html coverage, igual que los test. Para ello tenemos que hacer lo siguiente:

* Añadir al archivo .coverage: 
```
[run]
plugins = Cython.Coverage
```
* Después corremos la siguiente linea de código en la terminal ```cython  --annotate-coverage coverage.xml  package/mymodule.pyx``

# Strings

```
# distutils: language = c++

from libcpp.string cimport string

def get_ustrings():
    cdef string s = string(b'abcdefg')

    ustring1 = s.decode('UTF-8')
    ustring2 = s[2:-2].decode('UTF-8')
    return ustring1, ustring2
```

# Funciones y decoradores de Cython
* cython.compile(): nos dice si es compilado o no
* cython.declare(): nos ayuda a declarar variables -> ```x = cython.declare(cython.int)```
* @cython.cclass: declaramos una clase es como cdef class
* @cython.locals(): especificamos los tipos de variables locales en el cuerpo de una función, incluso los argumentos:
```
@cython.locals(a=cython.long, b=cython.long, n=cython.longlong)
def foo(a, b, x, y):
    n = a * b
    # ...
```
* @cython.returns(<type>): especifíca el tipo que retorna la función
* @cython.exceptval(value=None, *, check=False): Chequea exceptciones
* @cython.inline(): declarar un inliner
* @cython.final(): termina la cadena de herencias. Esto además permite usar e @inline()

## Decoradores que mejoran la velocidad

* ```@cython.boundscheck(False)```: cython asume que todas las operaciones de indexing están realizadas correctamente
* ```@cython.wraparound(False)```: cython asume que no hay indexing negativo
* ```@cython.cdivision(True)```: si se ajusta a True, significa que no habrá
* ```@cython.initializedcheck(False)```: cython por defecto chequea que los memoryviews han sido inicializados cada vez que se hace una operación con ellos. Si se pone False, se desactiva este check

# Numpy

Se puede usar Numpy sin las especificaciones de Cython, pero perderemos velocidad, ya que Cython tiene adaptaciones de Numpy. Por lo tanto es recomendable adaptar nuestro código de Numpy como Cython quiere.