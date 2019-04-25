##### Miguel González Contreras

# Práctica 3 de SWAP

## Pasos a seguir

### 1. Explicación

En esta tercera práctica de SWAP vamos a crear dos máquinas que van a hacer de balanceador de carga para las Máquinas 1 y 2 creadas en la Práctica 1. En las máquinas que hagan de balanceador tendrá que estar desinstalado el servidor Apache para que así deje abierto a escucha por parte de nuestros balaceadores el puerto 80. En la primera máquina que vamos a crear (Swap_M3-b ) con ip *192.168.56.140* vamos a instalar *nginx* y en la segunda máquina (Swap_M3-c haproxy ) con ip *192.168.56.130* instalaremos *haproxy*. Tras hacer esto, crearemos una 5º máquina (Swap_M4 ) que ésta sí tendrá instalado Apache para hacer las llamadas a los balanceadores. Por lo que tendemos estas máquinas: 
 ![disposicion de maquinas](https://github.com/miguegonzalez/SWAP/blob/master/practica3/0.Disposicion_maquinas.png)

## 2. Nginx

Primero instalaremos el balanceador con la orden *sudo apt-get install nginx* y lo lanzaremos *sudo systemctl start nginx*. Tras esto, tenemos que editar o crear el archivo de configuración de nginx, por lo que escribiremos en nuestra máquina *sudo vi /etc/nginx/conf.d/default.conf* y escribiremos lo siguiente:

![Configuracion nginx](https://github.com/miguegonzalez/SWAP/blob/master/practica3/1.Configuracion_nginx.PNG)

Tras esto comprobamos que tenemos conexión desde la Máquina 3 a las máquinas 1 y 2. Para ello hacemos *curl* a las respectivas máquinas.

![Comprobacion funciona nginx](https://github.com/miguegonzalez/SWAP/blob/master/practica3/2.Comprobacion_funciona_nginx.PNG)  

Ahora vamos a comprobar que funciona correctamente nuestro balanceador Nginx haciendo *curl* desde nuestra 4ª máquina al dominio de nuestro balanceador, que, como había dicho antes, es *192.168.56.140*.

![curl nginx](https://github.com/miguegonzalez/SWAP/blob/master/practica3/3.curl_nginx.PNG)

Para realizar el siguiente ejercicio, previamente voy a hacer la comparación de cuánta CPU utilizan la máquina 1 y 2 con una breve carga de peticiones.

![htop sin peticiones](https://github.com/miguegonzalez/SWAP/blob/master/practica3/4.htop_sin_peticiones.png)

Ahora con la ayuda de la orden *ab* que pertenece a Apache Benchmark podremos lanzar 1000 peticiones en unos segundos al balanceador para que éste mande aproximadamente la mitad de las peticiones a la máquina 1 y la otra mitad a la máquina 2, con la CPU del balanceador  ocupada casi al 100%.

![nginx Apache Benchmark](https://github.com/miguegonzalez/SWAP/blob/master/practica3/5.nginx_ab.png)


