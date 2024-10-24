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

