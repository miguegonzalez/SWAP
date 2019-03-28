# Práctica 1 de SWAP

## Pasos a seguir

### 1. Configuracion de Red

Para empezar, debemos de crear dos máquinas virtuales. Yo las he llamado Swap_M1 y Swap_M2. Antes de instalar Ubuntu Server en ambas máquinas tenemos que configurar la red quedando de la siguiente forma:
![configuracion de red](https://github.com/miguegonzalez/SWAP/blob/master/practica1/1.Configuracion_Red.PNG)

### 2. Instalación de Ubuntu Server

La instalación de nuestra máquina virtual viene explicado perfectamente paso a paso en el siguiente enlace: [http://www.ubuntugeek.com/step-by-step-ubuntu-12-04-precise-lamp-server-setup.html](http://www.ubuntugeek.com/step-by-step-ubuntu-12-04-precise-lamp-server-setup.html)

### 3. Editar interfaces

Lo siguiente que tendremos que hacer una vez instalado Ubuntu server es editar la interface que están en la carpeta */etc/network/interfaces* . Podemos utilizar la orden: *sudo vi /etc/network/interfaces*

Aquí debemos añadir todas las lineas a partir del *"#The secundary network interface"* poniendo en la Máquina 1 y en la Máquina 2 diferentes valores para *address*. Yo he decidido que en la Máquina 1 sea de **192.168.56.110** y que en la Máquina 2 sea de **192.168.56.120**: 
![configuracion de interfaces](https://github.com/miguegonzalez/SWAP/blob/master/practica1/2.Editar_interface.PNG)

### 4. Ping M1 to M2

Para comprobar que todo va correctamente y que ambas máquinas las he configurado bien, he hecho ping entre M1 y M2 y viceversa. Como podemos observar, está configurado perfectamente.
![ping](https://github.com/miguegonzalez/SWAP/blob/master/practica1/3.Pin_M1_M2.PNG)

### 5. Conexión Curl

En este paso vamos a realizar una conexión mediante la orden Curl (la hemos tenido que instalar antes con *sudo apt-get install curl*). Tras esto, vemos lo que hay en el apache de la otra máquina.
![Conexion Curl](https://github.com/miguegonzalez/SWAP/blob/master/practica1/5.Conexion_Curl.PNG)

### 5. Conexión ssh

Por último, vamos a hacer una conexión ssh para introducirnos en la otra máquina. Como vemos en la imagen, desde Swap_M1 nos conectamos a **192.168.56.120** (address de Swap_M2) y desde Swap_M2 nos conectamos a **192.168.56.110** (address de Swap_M1)

![Conexion ssh](https://github.com/miguegonzalez/SWAP/blob/master/practica1/6.Conexion_ssh.PNG)
