# uWSGI
El uWSGI es el encargado de gestionar las entradas y las respuestas al socket  

Sus archivos se encuentran en /var/www/html/

## Comandos uWSGI
```systemctl daemon-reload```
```sudo systemctl start myproject```
```sudo systemctl enable myproject```
```sudo systemctl status myproject```
```sudo systemctl restart myprojec```

### Dash
1. para Dash hace falta poner debajo de 
```app = dash.Dash(__name__)```
2. hay que crear la variable server
```server = app.server```
no se nos pueede olvidar importarla y ponerla  en el siguietne archivo como es (el punto 3)


3. y luego cuando Gunicorn llame la aplicación tienes que llamar al server, en vez de a la app
```--workers 2 --bind unix:gfk_statistical_sifnificance.sock -m 007 app.app:server```


# NGINX
Es el encargado de hacer correr las aplicaciones y unirlas a la web  

Sus archivos se encuentran en /etc/nginx/

## Comandos  NGINX
```sudo systemctl start nginx```
```sudo systemctl enable nginx```
```sudo systemctl status nginx```
```sudo systemctl restart nginx```
```sudo systemctl status nginx```


# Detectar errores
cuandoo algo no funciona usamos esto para ver que sudece
```sudo less /var/log/nginx/error.log```
```sudo less /var/log/nginx/access.log```
```sudo journalctl -u nginx```
```sudo journalctl -u myproject```


# Actualizar una app ya subida
Una vez ya tenemos el código en el servidor, tenemos que seguir los siguientes pasos:
1. reiniciar el proyecto que estemos actualizando ```sudo systemctl restart elproyecto```
2. reiniciar nginx ```sudo systemctl restart nginx```
3. dar permisos a la carpeta html ```chmod -R +777 html/```

# Errores.

En oscasiones puede que subamos una nueva app y no funcione, si no es por el puerto, y además ejecutando el comando ```sudo systemctl status myproject`` te aparece un warning diciendo que los logs no están disponibles:
1. Reinicia el servidor
2. Una vez reiniciado da todos los permisos de nuevo a la carpeta html ```chmod -R +777 html/```



# Configuraciones


**Systemd para flask (Uwsgi)***

 
```
[Unit]

Description=Gunicorn instance to serve gfk_app_manager

After=network.target
 

[Service]

User=admingfk

Group=admingfk

WorkingDirectory=/var/www/html/gfk_app_manager

Environment="PATH=/var/www/html/gfk_app_manager/app_manager-venv/bin"

ExecStart=/var/www/html/gfk_app_manager/app_manager-venv/bin/gunicorn --workers 2 --bind unix:gfk_app_manager.sock -m 007 app:app


[Install]

WantedBy=multi-user.target
```
 

 

**Systemd para dash (Uwsgi)**

 
```
[Unit]

Description=Gunicorn instance to serve gfk_statistical_sifnificance

After=network.target

 

[Service]

User=admingfk

Group=admingfk

WorkingDirectory=/var/www/html/gfk_statistical_sifnificance

Environment="PATH=/var/www/html/gfk_statistical_sifnificance/stat_sig-venv/bin"

ExecStart=/var/www/html/gfk_statistical_sifnificance/stat_sig-venv/bin/gunicorn --workers 2 --bind unix:gfk_statistical_sifnificance.sock -m 007 app.app:server --timeout 120

 

[Install]

WantedBy=multi-user.target
```
 

 

**Nginx para Flask**

 
```
server {

    listen 80;

    server_name datasciencetools.gfk.es www.datasciencetools.gfk.es;

 

    location / {

       include proxy_params;

        proxy_pass http://unix:/var/www/html/gfk_app_manager/gfk_app_manager.sock;

 

    }

}
```
 

 

**Nginx para Dash**

 
```
server {

    listen 80;

    server_name 52.138.139.242 www.52.138.139.242;

 

    location / {

        proxy_read_timeout 300s;

        proxy_connect_timeout 75s;

        include proxy_params;

        proxy_pass http://unix:/var/www/html/gfk_statistical_sifnificance/gfk_statistical_sifnificance.sock;

    }

}
```