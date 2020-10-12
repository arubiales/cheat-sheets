1. Configuramos Mongo

Vamos a ```sudo nano /etc/mongodb.conf``` y allí ponemos:
* bind_ip = IP que queremos permitir. Si queremos permitir todos es: <0.0.0.0> Pero ya sabemos que esto es malo para la seguridad
* port = <27017> Que es el puerto que usa Mongo

Una vez hecho esto accedemos a la terminal de Mongo y una vez allí creamos un usuario así:

```
db.createUser({user:"user",pwd:"password",roles:[{role:"root"}])
```

1. Configuramos AWS
* Comprobamos cual es el grupo de seguridad de nuestra instancia para ello vamos en el panel de control de la izquierda a **instances** y en la tabla, en la columna **Security Groups** vemos el grupo de seguridad de nuestra instancia
* Despues volvemos al panel de control de la izquierda a **Security Groups** (no en la tabla, en el panel). Seleccionamos el grupo de seguridad al que pertenecia nuestra instancia, que lo habíamos vistor en la tabla en el anterior paso. Después de seleccionado le damos en **Acciones** a **editar**
* Ahora editamos las reglas, poniendo los puestos 27018 y 27017 en donde pone **type** tanto en **Cusstom TCP**, en **Port range** ponemos ambos puertos, en source ponemos ambos puertos y en **Source** ponemos la ip del ordenador desde el que nos queremos conectar y tabmién el **Secuirity Group ID** que lo puedes encontrar en los detalles del security Groiup que vimos anteriormente

En conclusión habrás creado 4 reglas, 2 para 27018 una con la ip del ordenador que quieres acceder, y otra con el id del security group y lo mismo para el 27017. Estos puertos son los dos que usa MONGO.



3. Usamos en python Pymongo correctamente

```
from pymongo import MongoClient

connection = MongoClient('mongodb://arubiales:2HIJOSdeputa@15.188.14.246:27017/admin')
```

Ponemos:
* Lo que vamos a usar ```mongodb://```
* usuario
* contraseña
* puerto de la instancia
* admin: ponemos esto porque sí y ya está
