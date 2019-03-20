# SWAP 18-19
## Práctica 1: Preparación de las herramientas

El objetivo de la práctica1 es configurar dos máquinas (servidores) con Ubuntu Server para trabajar en prácticas posteriores.

Para ello lo que he hecho ha sido la instalación de dos máquinas Ubuntu Server en VirtualBox, con las opciones de servidor LAMP (Apache,MySQL,PHP) y SSH.

Y a las cuales he llamado:

    ubuntuswap1
    ubuntuswap2

A ambas máquinas desde virtual box hay que añadirle un nuevo adaptador de red con la configuración de red interna. Las dos máquinas virtuales creadas tienen el mismo nombre de red interna. (intnet)

Como resultado en cada máquina aparecen dos interfaces de red:

    enp0s3 -> NAT
    enp0s8 -> RED INTERNA

Despues he asignado la IP fija a la interfaz enp0s8 con : **sudo vi /etc/network/interfaces**

A continuación, muestro las modificaciones en ambas máquinas.


![sudo vi swap 1](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/sudo%20vi%20etc_network_interfaces%20swap1.PNG)

![sudo vi swap 2](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/sudo%20vi%20etc_network_interfaces%20swap2.PNG)


Una vez hecho este cambio debemos reiniciar y obtendremos las siguientes interfaces de red.

Haciendo: **ifconfig -a**



![ifconfig swap 1](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/ifconfig%20swap1.PNG)

![ifconfig swap 2](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/ifconfig%20swap2.PNG)

Para ver que Apache está funcionando, usando un editor de texto plano, crearemos el archivo HTML llamado hola.html en el directorio /var/www/html. 

Y mediante curl accedemos a ambas máquinas remotamente.


![curl swap 1 a swap 2](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/curl%20desde%20swap1%20a%20swap2.PNG)

![curl swap 2 a swap 1](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/curl%20desde%20swap2%20a%20swap1.PNG)

Por último realizo la instalación de SSH y muestro la conexión entre máquinas mediante SSH (de una máquina a la otra):


![ssh swap 1 a swap 2](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/ssh%20desde%20maquina%20swap%201%20a%20swap%202.PNG)

![ssh swap 2 a swap 1](https://github.com/JairoLuisAbrilMoya/Practica1/blob/master/ssh%20desde%20maquina%20swap%202%20a%20swap%201.PNG)

















