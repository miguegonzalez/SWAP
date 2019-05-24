##### Miguel González Contreras

# Práctica 5 de SWAP

## Pasos a seguir

### 1. Explicación

En esta quinta práctica de SWAP vamos a realizar una conexión de base de datos entre dos máquinas distintas, una haciendo de maestro y la otra de esclavo, por lo que cuando hagamos cambios en la máquina maestro se realizarán automáticamente en la esclava.

### 2. Preparar la máquina maestro

Lo primero que vamos a hacer es crearnos la base de datos y la tabla siguiendo los pasos siguientes:

![Create tabla](https://github.com/miguegonzalez/SWAP/blob/master/practica5/1.Create_tabla.PNG)

![Create tabla2](https://github.com/miguegonzalez/SWAP/blob/master/practica5/2.Create_tabla_2.PNG)

Ahora debemos evitar que se acceda a la Base de datos y que cambien los datos. Para ello en la maquina maestro:

![Lock the table](https://github.com/miguegonzalez/SWAP/blob/master/practica5/3._Lock_the_table.PNG)

### 3. Replicar BD

Seguimos en la máquina maestro, y ahora debemos hacer una copia de seguridad del archivo .SQL generado. Para ello usamos las prden *mysqldump contactos -u root -p > /root/contactos.sql*:

![Guarda copida de tablas](https://github.com/miguegonzalez/SWAP/blob/master/practica5/4.Guarda_copia_de_tablas.PNG)

El siguiente paso es desbloquear las tablas haciendo *UNLOCK TABLES;* y ya podemos hacer la copia a la otra máquina (SLAVE) con la orden *scp*:

![Copy maquina](https://github.com/miguegonzalez/SWAP/blob/master/practica5/5.copy_maquina2.PNG)

### 4. Clonado de BD

En el maestro, lo primero es editar el archivo de configuración de */etc/mysql/mysql.conf.d/mysqld.cnf* , comentando *#bind-address 127.0.0.1* y quitar de comentado :

*log_error = /var/log/mysql-error.log*

*server-id = 1*

*log_bin = /var/log/mysql/bin.log*

Reiniciamos el servicio: */etc/init.d/mysql restart*

![Configuracion maestro](https://github.com/miguegonzalez/SWAP/blob/master/practica5/7.configuracion_maestro.PNG)

Hacemos lo mismo con el **esclavo** a diferencia de poner ***server-id = 2*** 

### 5. Creación y utilización del Esclavo

Creamos el usuario esclavo en nuestra base de datos:

![Creacion esclavo](https://github.com/miguegonzalez/SWAP/blob/master/practica5/8.creacion_esclavo.PNG)

![Creacion esclavo](https://github.com/miguegonzalez/SWAP/blob/master/practica5/8.1.creacion_esclavo.PNG)

Una vez creado el esclavo, nos vamos a la máquina esclavo (mysql) y añadimos:

*CHANGE MASTER TO MASTER_HOST='192.168.56.110', MASTER_USER='esclavo', MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=980, MASTER_PORT=3306;*

y para finalizar hacemos un *START SLAVE* para iniciarlo. Si en la máquina maestro hacemos un SHOW SLAVE STATUS veremos algo así:

![FUFA esclavo](https://github.com/miguegonzalez/SWAP/blob/master/practica5/11.FUFA_SLAVE.PNG)

Y para finalizar haremos un insert en el maestro y se verá replicado en el esclavo:



![Comprobacion](https://github.com/miguegonzalez/SWAP/blob/master/practica5/12.COMPROBACION.PNG)




























