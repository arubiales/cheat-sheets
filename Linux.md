
## Sintaxis básica
* df: te dices como está dividido el disco, o el directorio si se lo pasamos
* ln: sirve para crear enlaces, puede ser un acceso directo (con s) o un enlace duro (por defecto). El enlace duro es una copia del primero y todas las modificaciones que se hagan en la copia o en el original se verán reflejados en el otro fichero.
* du: Te dice el tamaño que usado por un archivo o directorio
* find: primero le fijamos la opción por la que queremos buscar:
    * name: por nombre case sensitive
    * iname: por nombre no case sensitive
    * type: por tipo, hay que especificar f/d
    * size: poner +/- (numero)(unidad k/M/G) 
* less: es la evolución del comando moder
* cut: para seleccionar y mostrar un texto
    * -c (pasamos el rango: 7-25. O posiciones individuales 5,14. O seleccionar rangos: 7-12, 15-25, 27-29)
    * -d (pasamos un caracter)
    * -f (pasamos las columnas que queremos que se muestren)
* grep: sirve para encontrar patrones dentro de un fichero de texto y devolver las lineas con dicho patrón
    * -v: muestra lo que NO coincide con el patron usado
    * -l: realiza una búsqueda sobre muchos ficheros
    * -w: busca una palabra concreta
    * -n: nos devuelve el número de linea
    * -i: para que no sea case sensitive
    * -r: para buscar en directorios y subdirectorios
* sort: ordenar
    * -r: para realizar una ordenación inversa
    * -n: para realizar el orden basandose en los números
    * -h: para magnitudes de tamaño en bytes, megas, gigas, etc.
* uniq: quita las lineas distintas que están seguidas
* wc: nos da las lineas, palabras y caracteres
* rev: convierte los últimos elementos de cada linea en los primeros

## Caracteres especiales
* el asterisco *: nos dice a la derecha cualquier cosa
* la interrogación ?: en ese lugar va un caracter aleatorio (es decir puede ser cualquiera)

## Usuarios y permisos
Un usuario siempre pertenece a un grupo o más
* groups: te dice los grupos que tiene un usuarios
* adduser: agregar un usuario
* chown: con el root podemos cambiar un usuario y un grupo,
    * para el usuario hacemos chown (usuario al que cambiar) (fichero o directorio a cambiar)
    * Para el grupo hacemos chown :(grupo al que cambiar) (fichero que cambiar)
* passwd: cambiar la contraseña
* chmod: para cambiar los permisos de un fichero, sería así chmod (+/-r/w/x) (nombre fichero)

## Redirecciones
* ">": nos permite cambiar la salida de algo, por defecto en linux es el monitor, podemos cambiarlo y que se guarde en un fichero por ejemplo (no redirige los errores)
* ">>": igual que el anterior, pero en vez de sobreescribir el archivo, añade un nuevo registro en la última fila (es decir no sobreescribe)
* "2>/2>>": igual que los anteriores, pero en este caso solo se redirigen los errores
* "&>/&>>": como los de antes pero en este caso redirige ambos, los errores y los no errores
* "<": este al contrario que los demas, coge una entrada. Linux por defecto coje el teclado, pero tú con este comando puedes elegir cualquier archivo como entrada
Por último también añadir que podemos desechar testo que no queremos~~****~~ enviandolo a /dev/null


# Terminal de ubuntu en Windows
## Trucos
ubuntu1804 config --default-user your_username: este comando ejecutado en el cmd hace que la terminal de ubuntu en windows se habrá con el usuario que deseemos (sin saber su contraseña) podemos abrir root
