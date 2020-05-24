# 1. Intro
Todas las clases tienen una serie de propiedades al ser creadas, como __name__, __dict__. Estas por ejemplo guardan el nombre y los atributos y métodos de dicha clase. Con esto se puede jugar

# 2. Decoradores

## Property.

Se puede utilizar para dos cosas:
    1. para llamar a atributos mediante el punto que ejecuten funciones. Por ejemplo si tengo una clase círculo y un método que sea area. El area de ese circulo podría ser un atributo, que consultas y calcula el area del circulo (pi * r**2) automaticamente cuando lo llamas, ya que es un atributo del circulo en sí, no un método que ejecutar
    2. Para realizar modificaciones sobre los Inits o validaciones coon el setter y el getter

## StaticMethod

No es recomendado su uso ya que hay otras formas de hacer lo mismo, sirve para fijar funciones dentro de una clase, de tal forma que están aisladas en esa clase y cuando las llamamos, tenemos que usar la clase como prefijo

## Classmethod

Son metodos de clase, lo que hacen es que cuando cambiamos los atributos o métodos con métodos de clase, cambian automaticamente en todas las instancias de esa clase

# Operadores.

Hay de todo, los que ya conocemos como __init__, __repr__, pero también operadores por ejemplo __mul__ __add__ con esto podemos cambiar el funcionamiento de los operadore. Con los operadore podemos modificar el funcionamiento de los operadores +, -, *. etc. para cambiar su significado, o realizar operaciones con listas, strings, clases, distintas, etc.

# Rich comparition
Los operadores >, <=, ==, != etc. También son métodos especiales que podemos cambiar

# Booleanos

Todos los objetos en Python tienen un valor booleano. Trodos los número que no sean cero son considerados, True y el 0 es False. Definiendo el método __bool__ podemos cambiar esto en los objetos.   

Si el objeto __bool__ no es definido (por defecto los objetos son True), python busca a __len__.  

la función "bool" llama a __bool__ por defecto, pero si no se encuentra, llama al método __len__

# Callables

hace que la clase pueda ser llamada desde la instancia, pudiendo instanciarla sin definir suis parámetros. Esto se puede usar para dos cosas:
* Crear funciones objeto que necesiten mantenimiento
* Crear decoradores clase

Vemos lo que hace la función partial:

# Del

Es un método que no se recomienda usar, porque no es determinista, lo que quiere decir que depende de otras cosas para que se ejecute realmente, por lo que no es bueno usarlo

# Format
Implementar nuestro propio format es difícil. Por eso no se recomienda hacerlo

# Single Inheritance

* La herencia de clase, sucede cuando una clase hereda atributos y métodos de otra clase. La clase padre, es de la que se acaba de heredar, pero en el caso de que esta clase padre, haya heredado de otra clase a su vez. Para la clase hija, esta clase de la que ha heredado el padre, sería un ancestro

* la función builtin issubclass() nos dice si una clase ha heredado de otra, aunque no sea directa (padre)



# El objeto clase

Cuando creamos una clase, siempre se hereda automáticamente de clase de python. Es algo que Python hace automáticamente. Esto es lo que hace que esa clase tenga los métodos especiales __name__, __new__, etc. Esta herencia se hace de "object"


# Delegating to parent con super

A veces cuando sobreescribimos métodos necesitamos delegar hacia detrás a la clase padre o a cualquier ancestro, para no tener que volver a escribir lo mismo, el ejemplo más común es con el __init__, para ello usamos super().method() y hace que se traiga el método del padre tal y como está, incluso aunque nosotros lo hayamos sobrescrito, vamos a ver un ejemplo muy sencillo  


Como podemos ver, super ha llamado al método padre y luego se ha ejecutado también el método hijo. Pero hay **que tener cuidado** ya que si ponemos .sing() sin el super, se creará una recursión infinita.  

Una cosa que nos puede sorprender, pero que es así, es que cuando se llama a un método padre con super y contiene un self, el self que usa es el del hijo

# Slots

cuando quieres crear muchas instancias de objetos, (por ejemplo arrays, o points, etc) Python tendría que crear muchísimos diccionarios para las variables de cada instancia. En este caso podemos usar los __slots__ que sustituye a los diccionarios.  

Cuando hacemos esto, __dict__ desaparece de la clase y otros métodos especiales, esto hace que haya un ahorro de espacio increible que puede llegar a ser de hasta 3 veces menos. Además de que aumenta la velocidad entre las operaciones.

## Desventajas
obviamente todo esto trae desventajas con respecto las clases sin __slot__:
* No se pueden añadir atributos a una instancia
* Puede causar muchos problemas en herencias
* No se pueden redefinir slots en las clases hijo

Es conveniente solo usarlo por motivos de almacenamiento o rapidez, pero si no, lo mejor es no usarlo

Podemos ver que no existe un diccionario, y no podemos consultarlo. Aunque podemos hacer un hibrido e incluirlo. Esto nos permite que podamos añadir atributos a las instancias.

# Descriptors

Hay cuatro métodos que hay que existen en el **protocolo de los descriptores** (no todos son necesarios):  

1. __get__: el get de siempre para obtener un atributo
2. __set__: Para fijar un atributo
3. __delete__: para borrar un atributo
4. __set_name__: 

También tenemos dos categorías de **descriptors**:

1. Los que implementan solo el método __get__ (llamados non-data descriptors)
2. Los que implementan __set__ y/o __delete__ (llamados data descriptiors)


La forma de escritura es la siguiente: "def __get__(self, instance, owner_class):" esto es así porque instance, es la instancia a la que pertenece. y owner class es la clase a la que pertenece. En el ejemplo de abajo podemos ver:

* La clase a la que pertenece sería Logger. Y la instancia es None, si es llamada desde la clase directamente
* la instancia a la que pertenece sería l

Es necesario que el método deba ser escrito así, para que sepa la instancia que está referenciandolo, si no, tendríamos un problema al almacenar datos, ya que si hay datos distintos con instancias distintas, deberíamos saber la instancia, para saber que dato es. De ahí que haga falta este tipo de sintaxis

# Referencias débiles y referencias fuertes
* Las referencias fuertes, es cuando asignamos una variable a otra variable
* Referencias débiles: es una referencia al objeto, pero no cuenta para el memory manager. De esta forma cuando eliminamos una variable que referencia a un objeto de manera fuerte (strong reference), si no hay otra variable que referencia a ese mismo objeto de manera débil (weak reference), quedará eliminado (siempre y cuando no haya otra variable con strong reference)  

Para crear referencias débiles, podemos usar el modulo de python weakref, por lo que nuestrars variables creadas de esta forma serán referencias debiles al objeto. **pero hay que tener cuidadio** porque si cogemos una referencia debil y la asignamos a otra variable sin usar weakref, se crea una referencia fuerte a dicho objeto!!

## Introducir weakreference en los descriptores
Lo que queremos hacer es crear un diccionario de weak references (para nuestras claves) para nuestros descriptores de datos, para ello uusaremos la función "WeakKeyDictionary"


# Enumerations
Todo se encuentra en el modulo "Enum" hayu muchos tipos especializados de enumeraciones. Enum es un objeto que se utiliza en clases, heredandolo!

En el ejemplo de abajo, podemos ver que:_
* Color es lo que conocemos como una "enumeration"
* Color.RED: es llamado "enumeration member"
* Los miembros tienen asociados valores
* el type de un miembre es la enumeración a la que pertenece (en este caso Color)

Con los enums, para la igualación, siempre queremos usar "is" en vez de "==". Aunque ambos funcionan, "is" es más rápido y además nos permite sobreescribir el método"=="  

Son iterables, por lo que podemos listarlos  

Una vez una enumeración ha sido declarada, la lista de miembros es inmutable, no se pueden añadir ni agregar nuevos miembros  

No se puede heredar de una clase a menos que no contengan miembros

Las enumeraciones son tipos especiales de clases

## Alias

Como hemos comentado, los enum, cada clave y cada valor son únicos. Sin embargo esto no es del todo cierto, ya que podemos tener valores iguales en distintas claves, por ejemplo:

En esta enumeración nosotros tenemos solo 2 miembros, el color red y el blue, el resto son conocidos como **"alias"**. Estos alias apuntan a rojo o a azul según su valor

## EXTENDER ENUMS
Como hemos dicho no se pueden heredar enums, salvo, que no definas ningún miembro dentro, en ese caso si puedes extenderlos (obviamente los enums son inmutables, luego una vez defines algo, ya no puedes cambiarlo)

## Automatic Values
Es una forma de que Python asigne valores automáticamente a los miembros de nuestra enumeración. Para ello utiliza un método especial definido en la clase "Enum", llamado _generate_next_value_. La implementación por defecto empieza en uno y va a avanzando, enum.auto() es el staticmethod que llama a este método especial y sus argumentos son:
* name: el nombre del miembro
* start: No necesitamos saber esto
* count: el número de miembros que han sido creados (incluidos los alias)
* last_values: una lista de los últimos valores entrados

**No se debe usar auto() para definir valores automaticamente a la vez que tú creas tus propios valores, o una cosa u otra**

# Metaprogramming
Es para realizar cambios mediante código, en el propio código. Algunos ejemplos de metaprograming son:
* Decorators: Utilizar los decoradores para cambiar una propia función

## Metaclases
Son usadas a la hora de crear las proopias clases, no funcionan muy bien con la herencia. De hecho no se recomiendan usar, salvo que sean estrictamente necesario, lo cual te darás cuenta en el momento. Normalmente se usan para crear paquetes y librerías, si es para crear aplicaciones, no las uses.


## __new__

* Es el método que construye las instancias de cualquier clase. Siempre está en los objetos.
* sus argumentos son la clase, y +args y **kwargs
* Los argumentos __init__ deben estar en __new__
* Este método es llamado por Ptython cuando se llama a la clase automáticamente

Con __new__ podemos modificar la creación de las instancias
