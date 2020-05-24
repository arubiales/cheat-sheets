# 1. Crear la base de datos

* Tenemos que crear la base con "declarative_base()"
* Crear el esquema de datos, con las clases, es decir las tablas
* crear el motor (normalmente sqlite)

# 2. Operaciones CRUD
para hacer cualquier operación, tenemos que abrir una sesión e importar las clases (los esquemas de datos uqe hemos creado) y rellenarlos

## Crear
Usando la clase la instanciamos y despues unsamos git y commit para persistir los cambios en la BB.DD

## Leer
Usando "all" "first" o "one". Con el all usamos un bucle for para ir iterando

## Actualizar
Simplemente filtramos el valor que queremos, cambiamos consultando el atributo de la clase y añadimos y comiteamos

## Deletear
Filtramos la fila que queremos, usamos "delete" y le damos a add y commit para persitir los cambios