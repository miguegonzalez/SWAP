##### Miguel González Contreras

# Práctica 4 de SWAP

## Pasos a seguir

### 1. Explicación

En esta tercera práctica de SWAP vamos a instalar el certificado para poder hacer conexiones por https. Para ello los pasos a seguir son los que explicaremos a continución.

### 2. Preparar las máquinas finales

Lo primero que vamos a hacer es generar el certificado SSL en nuestra máquina final (por ejemplo Swap_M1). Lo hacemos a partir de las siguientes órdenes:

*sudo a2enmod ssl*

*service apache2 restart*

A continuacion creamos el directorio **ssl** en */etc/apache2* y ejecutamos la siguiente orden:

*openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt*

Y a continuación rellenaremos una serie de datos.

![Edicion del archivo openssl](https://github.com/miguegonzalez/SWAP/blob/master/practica4/1.Edicion_del_archivo_openssl.png)



Con ésta conseguiremos crear tanto el certificado (.crt) como la contraseña (.key).

Ahora editamos el archivo de configuración con la orden *sudo vi /etc/apache2/sites-available/default-ssl.conf* y añadiremos debajo de **SSLEngine on** las dos siguientes líneas:

*SSLCertificateFile /etc/apache2/ssl/apache.crt*
*SSLCertificateKeyFile /etc/apache2/ssl/apache.key*

![Configuracion Apache](https://github.com/miguegonzalez/SWAP/blob/master/practica4/2.Configuracion_Apache.png)

Ahora activaremos el sitio default-ssl y reiniciamos apache con las órdenes:

*a2ensite default-ssl*
*service apache2 reload* 

En la segunda máquina (Swap_M2) copiamos estos ficheros en el mismo sitio. Por lo que después de crear el directorio **ssl** ejecutamos:

*rsync -avz -e ssh 192.168.56.110:/etc/apache2/ssl/ /etc/apache2/ssl/*

y haremos los mismos pasos que con la Máquina 1.

### 3. Preparacion del balanceador Nginx

En nuestra máquina que utilizamos como balanceador copiaremos los archivos del certificado de la máquina Swap_M1 (ya que queremos que tambien acepte la conexión secure) haciendo:

*rsync -avz -e ssh 192.168.56.110:/etc/apache2/ssl/ /tmp/* (sí, en este caso en el directorio **/tmp/** ).

Y ahora editaremos el archivo */etc/nginx/conf.d/default.conf* . Quedaría de la forma siguiente:

![Configuracion Nginx](https://github.com/miguegonzalez/SWAP/blob/master/practica4/3.Configuracion_Nginx.PNG)

Para finalizar reiniciaremos Nginx (*sudo service nginx restart*).

Ahora probaremos que va todo bien. Para ello utilizaremos **curl**.

![Certificate https](https://github.com/miguegonzalez/SWAP/blob/master/practica4/4.Certificate_https.png)

![Funciona http y https](https://github.com/miguegonzalez/SWAP/blob/master/practica4/5.Funciona_http_y_https.PNG)

### 4. Cortafuegos a través de iptables

Ahora toca configurar un cortafuegos. Lo haremos con reglas iptables para filtrar el tráfico desde nuestro balanceador a una de mis máquinas finales. Aquí os adjunto las reglas que he insertado en mi script.

![Script Iptables](https://github.com/miguegonzalez/SWAP/blob/master/practica4/6.script_Iptables.png)
