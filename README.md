# Tarea 4 SXE

## 1.Utilizar la imagen de Ubuntu , tag 22 e instala LAMP en dicho contenedor.

<details>
  
  <summary>Pasos antes de las descargas</summary>
 
- Para descargar la imagen de ubuntu con el tag 22 en mi equipo utilicé:
```bash
 docker image pull ubuntu:22.04
```
- Esto lo descargó de la librería y para comprobar que estaba en mi equipo: 
```bash
docker images
```
![Docker_Images](https://github.com/user-attachments/assets/5b07b765-5c5a-4978-879d-a8b66efb1765)

- Creo el contenedor y accedo a él directamente:
```bash
 docker run -it --name ubuntu22 -p 8000:80 ubuntu:22.04 /bin/bash
```
- Actualizo:
```bash
apt update
```
- Instalo systemctl para más adelante:
```bash
apt install systemctl
```
</details>

<details>

  <summary>Instalaciones</summary>  
  
<details>

  <summary>Instalación Apache2</summary>

  Se ejecuta el siguiente comando para instalar apache2:
  
```bash
 apt install -y apache2 apache2-utils
```
  </details>

<details>

  <summary>Instalación MariaDB</summary>

  Se ejecuta el siguiente comando para instalar mariaDB

```bash
 apt install -y mariadb-server mariadb-client
```
Inicializo el servicio de mariadb
```bash
 service mariadb start
```
Y para asegurar la ejecucuión de mySQL ejecuto:
```bash
mysql_secure_installation
```
Salida por pantalla:

![Salida](https://github.com/user-attachments/assets/dcfe5b18-e1bd-401b-87d7-f5fc014d87af)

</details>

  <details>

  <summary>Instalación PHP</summary>

  Se ejecuta el siguiente comando para instalar php:

```bash
 apt install -y php php-mysql libapache2-mod-php
```
Reinicio el servicio de apache2:
```bash
systemctl restart apache2
```
  </details>
  
</details>

<details>

  <summary>Comprobaciones</summary>

  <details>
  <br>
    <summary>Apache2</summary>
  Para comprobar si tenemos instalado apache y funciona correctamente se ejecuta:
    
```bash
service apache2 status
```
![Apache2service](https://github.com/user-attachments/assets/9dd8d522-5932-42b2-be93-b5ef063346a5)

  </details>

   <details>
    <br>
    <summary>MariaDB</summary>
    Para comprobar el estado de mariaDB y mySQL se ejecutan:
     
```bash
service mariaDB status
```     
![mariadb](https://github.com/user-attachments/assets/f5d9ba88-6a7d-436a-b095-ba4f8ac8c8f6)

```bash
service mySQL status
```
 ![sql](https://github.com/user-attachments/assets/761cc4bb-7f21-44a8-8535-b47f3f658425)
    
  </details>
  
   <details>
    <br>
    <summary>PHP</summary>

  Se ejecuta el comando:

```bash
echo "<?php phpinfo(); ?>" | tee /var/www/html/info.php
```
Se pone en el navegador el puerto en el que se inicio ubuntu acompañado de /info.php para ver el estado de php

```bash
http://localhost:8000/info.php
```

![Captura de pantalla de 2024-10-24 13-03-08](https://github.com/user-attachments/assets/38131264-27ed-4172-afad-c18acdd3589b)

  </details>
  
</details>

## 2. Instalar wordpress en el contenedor y comprobar que puedo acceder.

<details>

  <summary>Instalación dependencias de Wordpress</summary>

  Ejecutamos el siguiente comando con todas las dependencias(algunas ya están instaladas de apartados anteriores, pero se puede ejecutar todo el script sin problema).

  ```bash
apt update
apt install apache2 \
  ghostscript \
  libapache2-mod-php \
  mysql-server \
  php \
  php-bcmath \
  php-curl \
  php-imagick \
  php-intl \
  php-json \
  php-mbstring \
  php-mysql \
  php-xml \
  php-zip
```

</details>

<details>

  <summary>Preparación y descarga WordPress</summary>

  ```bash
# Primero se crea un directorio
mkdir -p /srv/www
#Se cambia el acceso al usuario
chown www-data: /srv/www
#Instalar curl
apt install curl
#Descargar WordPress 
curl https://wordpress.org/latest.tar.gz | tar zx -C /srv/www
```
Salida por pantalla:

![Curl](https://github.com/user-attachments/assets/60db4946-d9bb-4d62-b467-bc76312d348e)

</details>

<details>

<summary>Configurar Apache</summary>

Instalar nano:
```bash
apt install nano
```

Crear un documento de la siguiente manera
```bash
nano etc/apache2/sites-available/wordpress.conf
```
Y dentro se escriben las siguientes dependencias:
```bash
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
```

Y por último se habilita la página:


```bash
# Habilitar el sitio
a2ensite wordpress
# Habilitar la URL de reescritura
a2enmod rewrite
# Deshabilitar el predeterminado
a2dissite 000-default

# Puede que entre los pasos se necesite hacer lo siguiente:
service apache2 reload
service apache2 restart
```

Comprobación de si puedo acceder a WordPress desde el navegador con la IP y el puerto añadiéndole /wp-admin/setup-config.php:

![Word](https://github.com/user-attachments/assets/8515f0c3-d8ee-4ebb-bb0d-2cdaf87d3219)

</details>

<details>

<summary>Configuración de la base de datos</summary>
<br>
Primero entramos en la base de datos:

```bash
mysql -u root
```

Una vez dentro ejecutamos los siguientes comandos:
```bash
# Crear la base de datos
CREATE DATABASE wordpress;
# Crear usuario con la contraseña
CREATE USER wordpress@localhost IDENTIFIED BY '<your-password>';
# Dar permisos
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
    -> ON wordpress.*
    -> TO wordpress@localhost;
# Actualizar privilegios
FLUSH PRIVILEGES;
# Salir de la base de datos
quit
```
![BD](https://github.com/user-attachments/assets/9721787d-fdc2-48b7-be80-0ade16df7300)

</details>

<details>

<summary>Configurar WordPress para acceder a la Base de Datos</summary>
<br>
Primero se copia el archivo de configuración:

```bash
cp /srv/www/wordpress/wp-config-sample.php /srv/www/wordpress/wp-config.php
```
Luego se establecen los datos en el archivo wp_config.php


```bash
# Reemplaza el nombre de la base de datos
sed -i 's/database_name_here/wordpress/' /srv/www/wordpress/wp-config.php

# Reemplaza el nombre del usuario
sudo -u www-data sed -i 's/username_here/wordpress/' /srv/www/wordpress/wp-config.php

# Reemplaza la contraseña
sed -i 's/password_here/<your-password>/' /srv/www/wordpress/wp-config.php
```
A continucación se abre el archivo con nano:

```bash
nano /srv/www/wordpress/wp-config.php
```

Se cambian las claves por unas generadas aleatoriamente en https://api.wordpress.org/secret-key/1.1/salt/. Se cambian las siguientes líneas por las de la página:

![ss](https://github.com/user-attachments/assets/c61e0e9a-dafe-40d0-b6ae-bf0472c962c8)

Por último se entra en el enlace de localhost en el navegador

```bash
http://localhost:8000/wp-admin/
```
Desde aquí se configura toda la página con el nombre de sitio de la página,usuario, etc.

Y de esta forma ya tendríamos la página de WordPress lista para trabajar en ella.

</details>

