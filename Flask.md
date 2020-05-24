# 1. Crear una ruta
* Se hace con el decorador app.route("/aquí_va_la_dirección")
* Después ponemos una función con la que se crea la página

## La dirección puede ser dinámica por lo que dependerá de la situación o petición del cliente

**Una ruta dinámica tiene dos aspectos clave**
1. Una variable dentro de la ruta (app.route())
2. Un parametro pasado a la función

**En las funciones podemos aplicar operaciones como en cualquier función, indexación, otras funciones, etc.**

**También es importante saber que en el modo debug, cuando hay un error con el pin puedes displayear una consola y ver las variables**

# 2. Plantillas
Son páginas html que se pueden renderizar desde desde las funciones de Flask con la función **render_template()**

# 3. Jinja
Sirve para introducir lenguaje de python en archivos HTML. Así en las funciones podemos introducir tantas variables y lógicas como queramos que aparezcan en el HTML y también en el HTML podemos hacer tantas lógicas en python como queramos. Nos movemos en codigo python de la siguiente manera:
* {{ }} con ello indicamos que estamos visitando variables
* {% %} para bucles y condiciones, por ejemplo: 
{% for item in lista %}    
{{item}}  
{% endfor %}  

Jinja tiene pipes | y métodos iguales o muy parecidos a Python. Por lo que podemos cojer una variable y modificarla, por ejemplo:  

{{ name | capitalize }}

Aquí le decimos que coja la variable name y la capitalice  

# 4. Template inheritence (herencia de plantillas)
Sigue el mismo principio lógico que las clases, en vez de tener muchas plantillas parecidas con ligeros cambios, podemos usar una base y crear distintas plantillas, para ello:

1. Creamos una plantilla base
2. usamos {% block content %} y los estados {% endblock %} para crear la plantilla base
3. usamos {% extyends "plantilla_base.html" %} y {% block content %} {% endblock %} para extenderlo en otro hmtl

# 5. url_for()
* Le puedes pones una función y te lleva a la página de esa función
* También un archivo donde se encuentre una imagen y después con el parámetro filename decirle el nombre de la imagen por ejemplo
* Tienen que apuntar a funciones, no a webs.

# 6. Formularios
* Ponemos GET y POST en el app.route
* Con la sesión podemos consultarla y acceder a lo que ha puesto un usuario
* Al inicio se crea una clase formulario que será la encargada de desarrollar nuestro formulario
* Se rellena con Jinja el HTML
* Tienes validadores para los campos

# 7. BB.DD
Almacenan los datos necesarios para nuestra aplicación. con Flask_sqlalchemy podemos **heredar de las clases Model y de Flask_login UserMixin**. Estas bases de datos tienen varios atributos, métodos y funciones distintas:
* __tablename__: el nombre de la tabla
* is_activated: saber si un usuario está activado
* is_authenticated: saber si un usuario está autentificado
* check_password: comprobar la contraseña
* check_mail: comprobar el email

**Programación para empezar la base de datos**:  
db = SQLAlchemy(app)  
Migrate(app, db)  
  
Con esto creamos la base de datos y la migramos, para que cualquier cambio que hagamos, se ejecute en dicha base de datos


# 8. Logins
Flask_login Tiene varios temas para manejar los usuarios y métodos, se inicia así:  

login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = 'users.login'

Donde la login.view es igual a la función que renderiza la template de login.

Esta aplicación nos permite establecer con seguridad, cuando un usuario está logeado o no, y facilita su manejo

a partir de aquí podemos ejecutar varios métodos:
* @login_manager.user_loader: es obligatorio definir este método cuando se logea el usuario, ya que nos dice si un usuario está logeado
* @login_required: esto le dice a una página que es necesario estar logeado para acceder

# 9. Blueprints
Sirven para poder organizar mejor nuestra aplicación y nuestras carpetas, con ello puedes separar las distintas rutas, según pertenezcan a distintas partes de la aplicación

# 10: CSS
* si quieres crear una clase, se hace con un punto, si quieres crear un id se hace con una almohadilla
* para realizar atributos como hover, se utilizan los 2 puntos después de la clase (la clase tiene que haver sido creada previamente)
* Recuerda que **puedes llamar al style en cada etiqueta** o **introducir span**

# 11. Heroku
Sirve para crear entornos de aplicaciones en web de forma rápida y efectiva. Simplificandolo todo. Es gratuito pero si quieres escalarlo es de pago.
Se basa en los siguientes ficheros necesarios que deben de star en la raiz:
* Procfile: el archivo que le dice con gunicorn como debe de iniciarse
* requirements.txt: con pip freeze > requirements.txt congelamos todas las bibliotecas necesarias para la aplicación

Para subir los ficheros, hacemos igual que con git, pero al pushear ponemos "git push heroku master". Una vez hecho esto, heroku hará todo lo demás

# OTROS TEMAS
* Flask va a buscar los templates a la carpeta templates
* Para hacer las página interactivas necesitas JS
* Puedes pasar los datos que quieras a la template, a través del render template para poder acceder a ellos
* JavaScript es un lenguaje fundamental para esto
* Los directorios suelen tener una estructura y hay archivos por defecto
  * Directorios por defecto
    * Templates: es donde se guardan los htmls
    * users: el manejo de usuarios
    * temp: archivos temporales
    * static: donde va el css, bootstrap y todos los paquetes fuera de python que ayudan a la web
    * index: la página de inicio
    * error_pages: las páginas de error
    * models: son los modelos de las bases de datos, puede haber tantos, como se necesiten.
  * Ficheros por defecto
    * La aplicación debe estar en el directorio raiz con nombre "app.py"
    * configuration: debe estar en el directorio raiz junto con app.py
    * requirements.txt: son las bibliotecas requeridas 
    * structure.txt: es la extructura de carpetas del proyecto, te ayuda a localizarlos todo
    * Procfile: el archivo que se encarga de hacer el deploy en Heroku
    * primer __init__.py es donde se escribe el core de la aplicación junto a los directorios de template, users, static, etc.
    * forms: para los formularios, puede haber tantos como se necesiten.


# COSAS QUE SIEMPRE SE HACEN
Inicialización de una app flask
1. export FLASK=app/app.py #el archivo donde se encuentre
2. flask db init
3. flask db migrate
4. flask db upgrade