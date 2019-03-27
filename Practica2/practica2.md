# SWAP 18-19
## Práctica2: Clonar la información de un sitio web
El objetivo de la práctica2 es aprender copiar archivos con ssh, clonar contendido entre máquinas, configurar ssh para acceder a máquinas remotas sin contraseña y establecer tareas con cron.

## Copiar archivos con SSH
Para copiar archivos entre una máquina y otra mediante _ssh_ usamos el siguiente comando:

`tar czf - directorio_1 | ssh javi@192.168.1.100 'cat > ~/tar.tgz'`

Con esto lo que conseguimos es comprimir el contenido de 'directorio_1' y enviar el archivo comprimido a la máquina destino.

![sudo vi paso1_1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso1.1.PNG)
![sudo vi paso1_2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practica2/imagenes/paso1_2.PNG)
