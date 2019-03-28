# SWAP 18-19
## Práctica2: Clonar la información de un sitio web
El objetivo de la práctica2 es aprender copiar archivos con ssh, clonar contendido entre máquinas, configurar ssh para acceder a máquinas remotas sin contraseña y establecer tareas con cron.

## Copiar archivos con SSH
Para copiar archivos entre una máquina y otra mediante _ssh_ usamos el siguiente comando:

`tar czf - directorio_1 | ssh jairoantonio2@192.168.1.100 'cat > ~/tar.tgz'`

Con esto lo que conseguimos es comprimir el contenido de 'directorio_1' y enviar el archivo comprimido a la máquina destino.

![  paso1_1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso1.1.PNG)

![  paso1_2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso1_2.PNG)

Pero esto no nos sirve para sincronizar grandes cantidades de información, luego usamos rsync.

## Instalando y usando la herramienta rsync

Para instalar rsync en nuestras máquinas hay que ejecutar el siguiente comando:

`sudo apt-get install rsync`

Para comprobar que lo hemos instalado correctamente al igual que con apache ejecutamos en ambas máquinas con el siguiente comando:

`rsync --version`

![  rsyncswap1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/versionrsyncswap1.PNG)

![ rsyncswap1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/versionrsyncswap2.PNG)

Una vez hecho esto, vamos a copiar el directorio _/var/www/_ de la maquina1 a la maquina2, para ello ejecutamos en la maquina2 el siguiente comando:

`rsync -avz -e ssh jairoantonio2@192.168.1.100:/var/www/ /var/www/`

Como podemos ver antes de hacer el `rsync` en la maquina2 no hay nada en el directorio _/var/www/html_ , una vez hecho vemos como copia en la maquina2 el directorio y todo el contenido que hay en él.

![paso3_1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso3_1.PNG)

![ paso3_2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso3_2.PNG)

Si queremos ignorar ciertos directorios en nuestra copia podemos usar el parámetro `--exclude` y con el parámetro `--delete` los ficheros que se hayan eliminado en la maquina1 tambien se eliminan en la máquina2.

Para ello debemos ejecutar:

`rsync -avz --delete --exclude=**/stats --exclude=**/error --exclude=**/files/pictures -e ssh jairoantonio2@192.168.1.50:/var/www/ /var/www/`

![ paso4](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso4.PNG)

## Acceder a SSH sin contraseña

Para poder acceder mediante SSH de una máquina a otra, lo que tenemos que hacer es generar un par de claves RSA. Para ello ejecutamos el comando:

`ssh-keygen -b 4096 -t rsa`

Lo cual muestra por pantalla:

![ keygen](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso5sshkeygen.PNG)

Tras hacer esto tenemos que copiar la llave pública a la maquina1. Para ello:

`ssh-copy-id jairoantonio2@192.168.1.100`

![ paso5_2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso5_2.PNG)

Para comprobar que la copia se ha realizado correctamente nos conectamos a la maquina1 mediante SSH desde la maquina2 y vemos como esta vez no nos pide contraseña.

![ paso5_3](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso5_3.PNG)

## Actualizar el contenido de /var/www/ con contrab

cron es un administrador de procesos en segundo plano que ejecuta procesos en el instante indicado en el fichero crontab. Podemos agregar nuevas tareas a cron para automatizar algunos procesos.

Por ello editamos el archivo _/etc/crontab_ mediante:

`sudo nano /etc/crontab`

Una vez dentro de ese archivo, añadimos la siguiente linea:

`3 */3 * * * root rsync -avz -e ssh jairoantonio2@192.168.1.50:/var/www/ /var/www`

Con _3 */3 * * *_ indicamos que se ejecute la orden ssh en el minuto 3 cada 3 horas.

El resultado del fichero crontab es el siguiente:

![  paso6](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso6.PNG)

