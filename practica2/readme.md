# Práctica 2 de SWAP

## Pasos a seguir

### 1. Instalación de rsync

Para empezar esta práctica, tenemos que instalar rsync y darle todos los permisos a la carpeta /var/www:  
![instalacion de rsync con permisos](https://github.com/miguegonzalez/SWAP/blob/master/practica2/1.Instalacion_Rsync_Permisos_en_Carpeta.PNG)

## 2. Copia por ssh

Lo primero que se nos pide en esta práctica es copiar un archivo de una máquina a otra mediante ssh. Lo que yo he hecho ha sido comprimir la carpeta */var/www/html* de mi máquina Swap_M2 y enviarla a la carpeta principal de mi máquina Swap_M1   

![Copia por ssh](https://github.com/miguegonzalez/SWAP/blob/master/practica2/2.Copia_Por_ssh.PNG)

## 3. Ssh sin contraseña

Lo segundo que se nos pide es configurar ssh sin necesidad de contraseñas para tener una conexión entre las dos máquinas. Para ello, hay que generar un archivo de contraseña de ssh para después enviarlo a la máquina a la que quiero acceder por ssh. Todo este proceso se ha hecho de la siguiente forma:

![SSH Keygen](https://github.com/miguegonzalez/SWAP/blob/master/practica2/3.ssh-keygen.PNG)

Y después hay que hacer *ssh-copy-id 192.168.56.110* en la máquina Swap_M2 (en mi caso) para poder acceder a la máquina Swap_M1 sin necesidad de contraseña.

![conexion con M1](https://github.com/miguegonzalez/SWAP/blob/master/practica2/4.Conexion_con_M1.PNG)

## 4. Clonación rsync

Lo siguiente que tenemos que hacer es una sincronización de un directorio en ambas máquinas, osea, tener los mismos archivos y directorios en la máquina 1 y en la máquina 2. Lo hacemos de la siguiente forma:

![clonacion rsync](https://github.com/miguegonzalez/SWAP/blob/master/practica2/5.Clonacion_rsync.png)

Tras esto, vemos como todos los archivos de Swap_M1 que estaban en */var/www/* se han clonado a Swap_M2.

## 5. Crontab

Por último, tenemos que configurar crontab para que haga una tarea cada x tiempo. En mi caso, lo he configurado para que clone el directorio */var/www* de mi máquina Swap_M1 con el de mi máquina Swap_M2 todos los días a las 18:00h. Tras esto, se debe de reiniciar el crontab con *systemctl restart cron* para que funcione.

![crontab](https://github.com/miguegonzalez/SWAP/blob/master/practica2/6.crontab.png)
