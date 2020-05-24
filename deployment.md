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
```sudo journalctl -u nginx```sudo
```sudo journalctl -u myproject```


# Actualizar una app ya subida
Una vez ya tenemos el código en el servidor, tenemos que seguir los siguientes pasos:
1. reiniciar el proyecto que estemos actualizando ```sudo systemctl restart elproyecto```
2. reiniciar nginx ```sudo systemctl restart nginx```
3. dar permisos a la carpeta html ```chmod -R +777 html/```






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


