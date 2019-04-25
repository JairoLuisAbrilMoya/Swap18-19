# SWAP1718
### Práctica3 : Balanceo de carga

El objetivo de la práctica 3 es aprender a configurar un balanceador que reparta la carga entre varios servidores finales conectados en una red, para solucionar el problema de la sobrecarga de los servidores. Así conseguimos una infraestructura redundante y de alta disponibilidad.

Para ello los pasos realizados son los siguientes:

##### Red de las máquinas

Dejar claro que el tipo de red que tengo entre las máquinas es: red interna.
La dirección de red en la que están todas las máquinas es: 192.168.1.1

En particular:

ubuntuserver1       :   192.168.1.101
ubuntuserver2       :   192.168.1.100
balanceador  nginx  :   192.168.1.107
balanceador haproxy :  192.168.1.111
peticiones          :   192.168.1.110

### Balancear la carga usando nginx

Para instalar nginx hemos ejecutado el siguiente comando de instalación:

**sudo apt-get install nginx**

Iniciamos el servicio y comprobamos que está activo:

![active y status nginx](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/activeystatusngingx.png)


Una vez lo tenemos instalado, pasamos a configurar nginx ya que la configuración que viene por defecto no nos sirve.
Para ello modificamos el fichero de configuración **/etc/nginx/conf.d/default.conf** añadiendo lo siguiente:

![configuracion nginx](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/configuracionnginx.PNG)

Para que nginx deje de funcionar como servidor y empieze a trabajar como balanceador tenemos que modificar el fichero nginx.conf que se encuentra en la ruta /etc/nginx/nginx.conf. En él, tendremos que comentar la siguiente línea:

 **#include /etc/nginx/sites-enabled/*;** 
 
 ### Configurar nginx para balanceo ponderado

Usamos este tipo de balanceo si sabemos que alguna de las máquinas finales es más potente, para ello modificamos la definición del “upstream” de una de las máquinas para pasarle más tráfico que al resto de las del grupo. Para que esto sea posible tenemos el modificador “weight”, al que le damos un valor numérico distinto de 1 (valor por defecto).

Cambiando el nivel de carga. (La máquina1 tiene el doble de capacidad que la máquina2)

![cambiando nivel de carga nginx](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/cambiandonivel%20de%20carga%20ngnix.PNG)

Para comprobar las diferencias entre los balanceos, hago peticiones mediante curl (desde la máquina peticiones) a la ip del balanceador. (192.168.1.107)

#### Funcionamiento balanceo Round-Robin

![balance round robin](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/balanceo%20round%20robin.PNG)

#### Funcionamiento balanceo ponderado

![balanceado por ponderado](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/balanceo%20por%20ponderado.PNG)


Como podemos observar en el primer caso reparte la carga por igual, hace una petición a cada máquina. En cambio en el segundo caso, como hemos dicho que la maquina1 tiene el doble de capacidad que la maquina2 en este caso el balanceador hace 2 peticiones a la maquina1 y 1 a la maquina2.

## Balancear la carga usando haproxy

Para instalar haproxy he ejecutado:

**sudo apt-get install haproxy**

Iniciamos el servicio y comprobamos que está activo:

![activado e instalado haproxy](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/haproxyactivado%20e%20intsalado.PNG)

Una vez instalado, debemos indicarle las IPs de las dos máquinas servidoras.
Para ello, modificamos el fichero **/etc/haproxy/haproxy.cfg** añadiendo a la configuración de haproxy lo siguiente:

![configuracion haproxy](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/configuracionhaproxy.PNG)

### Comprobar que funciona

Para lanzar haproxy una vez hemos cambiando su configuración ejecutamos el siguiente comando:

**sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg**

Al realizar este comando nos aparecen tres Warnings debido al desuso de timeout que usamos:

![warnings haproxy](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/warning%20del%20comando%20debido%20al%20desuso%20de%20timeout.PNG)


Y a continuación le mandamos peticiones y vemos como equireparte la carga entre ambas máquinas finales.

![curl maquinas 1 y 2 haproxy](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/curl%20de%20maquinas1y2%20para%20haproxy.PNG)


## Someter a una alta carga a la granja web
### nginx

Para probar nuestra granja web con un balanceador nginx, vamos a lanzar 90000 peticiones de 10 en 10 con apache benchmark a nuestro balanceador, pidiendo la página hola.html

**ab -n 1000 -c 10 http://192.168.1.107/hola.html**

#### Comprobando con top

![top nginx balanceador](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/topbalanceadornginx.PNG)

#### Salida del ab

![salida nginx ab](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/salida%20ab%20nginx.PNG)

En este caso, he realizado 1000 peticiones al servidor, donde ha tardado 1.991 segundos en hacer el test completo, no ha habido peticiones fallidas lo que quiere decir que el servidor no se ha saturado al realizar este test.

Podemos observar como es capaz de atender 502.23 peticiones por segundo, tardando unos 19.911 ms de media por petición y a una velocidad de transferencia de 168.39 Kbytes/sec.


### haproxy

Para poner a prueba nuestro servidor con haproxy como balanceador de carga hemos hecho 1000 peticiones haciendo las peticiones de 10 en 10. (Igual que con nginx)

**ab -n 1000 -c 10 http://192.168.1.111/hola.html**

#### Comprobando con top

![top balanceador haproxy](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/topbalanceadorhaproxy.PNG)

#### Salida del ab

![salida haproxy ab ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/salida%20ab%20haproxy.PNG)

En este caso, con las mismas peticiones(1000), ha tardado 1.759 segundos en hacer el test completo, no ha habido fallos, luego el servidor no se ha saturado.

En este caso se han realizado 568.53 pet/s, tardando 17.589 ms de media por petición y a una velocidad de transferencia de 190.71 Kbytes/sec


### Comparación de balanceadores

Como hemos observado los dos balanceadores han podido responder a todas las peticiones y ninguno se ha saturado, pero vemos un mejor rendimiento en haproxy debido a que los tiempos de respuestas son menores, tiene mayor tasa de pet/s y una mayor velocidad de transferencia.

Aquí podemos ver una comparación de tiempos:


