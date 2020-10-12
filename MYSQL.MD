# 1. Instalamos

sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation

*Despues: sudo mysql_secure_installation (esto es para la configuración)

# 2. Configuración de root
* Accedemos: sudo mysql
* ponemos una password a root: ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
* actualizamos los privilegios: FLUSH PRIVILEGES;

sudo mysqladmin -p -u root version

# 3. Crear usuario y base de datos dar privilegios
* CREATE USER 'arubiales'@'%' IDENTIFIED BY 'password';
* GRANT ALL PRIVILEGES ON *.* TO 'arubiales'@'%' WITH GRANT OPTION;

# 4. Abrir puerto AWS
* puerto: 3306
* ip: del ordenador que nos vamos a conectar

Y también abrimos otro con el mismo puerto y la fuente de nuestro aws (como en mongo)

# 5.
vamos a ```etc/mysql/mysql.conf.d/mysqld.cnf``` y en bind_ip, ponemos la ip que se va a conectaar
