# GIT 

* reset: para quitar los archivos añadidos al stage. Poniendo el archivo en concreto nos quita ese archivo
    * --soft id_commit: volvemos al commit indicado sin borrar lo avanzado
    * --hard id_commit: vamos al commit indicado, borrando todo lo siguiente, o avanzado hacia el futuro en el caso de que hayamos borrado algo que no queríamos


* reflog: es un historial de todo lo que se ha realizado en el repositorio, incluyendo resets hards y todo tipo de cosas


* diff: para ver los cambios que hay en el documento desde el último commit (sin que haya habido ningún commit, ni add, ni nada)
    * --staged: Si hemos hecho add, podemos ver los cambios sobre el stage.
    * nombre_rama1 nombre_rama2: nos da los cambios entre las dos ramas


* commit: sabemos lo que es el commit, vamos a ver más opciones sobre el
    * --ammend: sirve para actualizar un mensaje relizado con -m (cuando hayas escrito mal el mensaje)
    
    
* show: sirve para mostrar información adicional de los comandos
    * tag nombre_tag: muestra toda la información sobre ese tag


## Ramas

* branch: nos indica las ramas que hay y en cual nos encontramos
    * si ponemos un nombre después, creamos una nueva rama
    * -d nombre_rama: eliminamos la rama (esto es una buena práctica eliminar una rama secundaria cuando la integramos con la principal)

* checkout nombre_rama: nos cambia a esa rama
    * -b nombre_rama: nos crea una rama y nos mueve a ella

* Merge: es cuando dos ramas convergen en una sola, hay varios escenarios que se pueden dar.
    1. Fast-forward: cuando en la rama principal no ha habido ningún cambio y sí en la secundaria. Por lo que los cambios de la rama secundaria pasarán a la principal
    2. Uniones automáticas: si hay cambio en la rama principal, pero no en los mismos archivos que en la secundaria, por lo que GIT las une sin problema
    3. Unión manual: es cuando han cambiado los mismos archivos, por lo que git nos pide que arreglemos el conflicto seleccionando una de las dos.


## Tags
Son usados para marcar las versiones de los ejecutables. Es decir cuando has acabado un proyecto y ya se puede al menos usar, por ejemplo Tensorlfow 1.0.0 alpha

* tag nomble_del_tag: para crear un tag
    * -d nombre_tag: borrar un tag
    * -m mensaje_al_tag: sirve para poner un mensaje para el tag (al igual que con los commits)
    * si ademas ponemos el id de un commit podemos crear un tag para ese commit pasado
    
* Las versiones, por ejemplo v1.0.1 Alpha significan lo siguiente.
    * Primer número: indica que es un cambio muy relevante en las funcionalidades del paquete
    * segundo número: indica que se han añadido funcionalidades o cambios menores a un paquete ya existente
    * tercer número: se usa solo para corrección de errores, fallos o bugs detectados una vez se ha lanzado el paquete
    * Alpha: indica que la versión es inestable y que por tanto te encontraras con errores
    * Beta: indica que la versión funciona bien la mayoría de las cosas, pero que puede que haya algún error en algunas funcionalidades
    
    
## Rebase
sirve para unir, separar, ordenar o corregir commits

* En el caso de que tengamos una rama master y otra rama secundaria, nos permite, poner los commits de la rama secundaria al final.
 Cuál es la diferencia con el Merge, pues que el merge, simplemente crea un commit de unión desapareciendo todos los commits de la rama
 secundaria, mientras que este, añade todos los commits a la principal, por lo que sigues teniendo el registro. Para hacer esto usamos:
    1. Primero hacemos: rebase nombre_rama_a_la_que_queremos_agregar (en este caso sería master)
    2. Segundo: checkout nombre_rama_principal (master)
    3. tercero: merge nombre_de_la_rama_que_queriamos_agregar (aquí ya hemos acabado de agregar la rama secundaría a la principal)

 
* Squash: Esto lo que hace es unir mensajes de commit que están consecutivos y son inecesarios una misma rama (por ejemplo, cambio readme, cambio readme1, cambio readme2, etc.)
    1. Primero: rebase -i HEAD~4: (es un rebase interactivo) esto indica que quiere hacer un rebase desde el head los últimos 4
    2. Despues en la pantalla cambiamos "pick" por "s" a los commit que queramos hacer squash.
    3. por último nos pedirá un mensaje para el nuevo commit (el -m)
    
* Edit: sirve para separar un commit en dos
    1. Primero: rebase -i HEAD~3: (rebase interactivo): indicamos que queremos hacer un rebase interactivo de los últimos 3
    2. Segundo: ponemos la "e" en los que queremos editar
    3. Tercero: hacemos git reset Head^ (con esto reseteamos el último cambio sin destruirlo). Ahora si hacemos git s, veremos que están los archivos fueras del stage
    4. Cuarto: hacemos add y commits tantos como queramos. Así habremos separado un commit en varios.
    5. Quinto: por último hacemos git commit rebase --continue (con esto acabamos el rebase interactivo y cerramos) y así habremos separado un commit en varios


## Stash (esto no es muy necesario por ahora)
Es para cuando estás realizando cambios en un repositorio, y necesitas de repente lanzar algo a producción en la última versión estable, pero tampoco quieres perder los cambios de lo que has avanzado.
Lo normal es que cuando retomes el trabajo elimines el último stash (e)

* stash: enviamos al stash, todo lo que se ha realizado hasta ahora
    * list: para saber todo lo que hay en el stash
    * pop: para recuperar los cambios que se habían realizado y enviado al stash (recupera y elimina el último stash)
    * drop: para borrar el stash
    

# GITHUB
Te ofrece varias cosas como comentar commits, crear archivos, cambiar nombres de archivo, etc. Todo en interfaz gráfica.  
Cuando hacemos cambios aquí hay que hacer pull para actualizar nuestro local  
Nos permite hacer comentarios en los commits entre compañeros  
En Github si eres colaborador puedes hacer lo mismo que si hubieras creado el repositorio (casi lo mismo)


## Tags
* push --tags: no sube automaticamente los tags con el push, hay que hacerlo a parte. por ello usamos esto

## Fetch
Se usa cuando has hecho cambios en tu repositorio local sin haber hecho un pull de tu repositorio en cloud. Lo que puede suceder es que se realice un commit
con merge y quede registrado uno delante de otro, o que entremos en una situación de conflicto si hemos modificado lo mismo

* fetch: simplemente esto y hacemos el fetch

## Raw, Blame, History
* Raw: para ver un archivo en formato raw
* Blame: para ver como fueron afectando los distintos commits a un archivo, nos permite ver los cambios que han ido sucediendo con los commits
* History: Vemos que coomits afectaron a un archivo en particular 

## Fork y pull request (esto pertenece solo a github)
Un **fork** toma el repositorio original y lo clonas a local y en nuestra cuenta de github, es como una rama virtual que se crea a partir de ese proyecto que solo tu
puedes verla pero en el futuro la podrías unir a la master. Pero no podemos actualizar el original porque ni somos los dueños, ni colaboradores
para ello necesitamos hacer un **pull request**. Esto sería enviarle los cambios a los dueños del proyecto para que ellos tengan la posibilidad de aceptarlos  
  
Para hacer un pull request lo tienes en la pantalla principal "New pull request" y en la siguiente pantalla se vera los cambios que afectarán al fork de la rama master

# COSAS MÁS USADAS

## MERGE COMMITS EN 1

1. Cuando tengas todo tanto en github como en git al día. ejecutar ```git rebase -i HEAD~n``` donde *n* es el número de archivos que queremos retroceder




