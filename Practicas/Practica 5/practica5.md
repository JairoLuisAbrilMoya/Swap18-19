## SWAP1718
### Práctica5 : Replicación de bases de datos MySQL

Los objetivos de la práctica 5 son: clonar manualmente BD entre máquinas y configurar la estructura maestro-esclavo entre dos máquinas para realizar el clonado automático de la información.

### Creación de una BD e inserción de datos

En todo momento usaremos la interfaz de línea de comandos del MySQL: mysql -uroot -p

Tras esto, crearemos una BD, junto con una tabla, tal y como se ve en la siguiente figura:

![mysql-u root -p](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/mysql%20-uroot%20-p.PNG)

A continuación insertamos algún dato en la BD que acabamos de crear, para tener algo de lo que hacer una copia de seguridad. En nuestro caso, insertaremos en la tabla Pepe y su número de teléfono.

![insert pepe y tlf](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/insertar%20pepe%20y%20tlf.PNG)

### Replicar una BD con mysqldump

Una vez terminada la inserción de datos, para poder hacer la copia de seguridad de forma correcta y que no surjan problemas porque alguien acceda a la BD mientras se está haciendo el volcado con mysqldump, tenemos que bloquear la BD.

Para ello, volveremos a entrar en el terminal de MySQL y ejecutaremos la siguiente orden:

FLUSH TABLES WITH READ LOCK;

Ahora ya sí podemos hacer el mysqldump para volcar los datos. Y a continuación desbloqueamos las tablas.

![mysqldump](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/mysqldump.PNG)

Una vez hecho esto, vamos a la máquina2 para copiar el archivo .sql con todos los datos salvados desde la máquina1.

![copia en m2 del archivo sql](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/copiaenm2delarchi%20sql.PNG)

Además es necesario crear en nuestra máquina esclavo (máquina2) la BD correspondiente ya que el mysqldump no incluye ese archivo la sentencia para crear la BD.

Para ello primero creamos la BD y luego restauramos los datos contenidos en la BD (se crean las tablas en el proceso).

![creamos en la m2 la BD](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/creamos%20en%20la%20m2%20la%20BD.PNG)

![restauramos los datos contenidos en la BD](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/restauramos%20los%20datos%20contenidos%20en%20la%20BD.PNG)

### Replicación de BD mediante una configuración maestro-esclavo.

Lo hecho anteriormente está bien, pero podemos mejorarlo configurando un proceso demonio automático, mediante el cual al realizar un cambio en la base de datos maestro (máquina1) se vea directamente reflejado en el esclavo (máquina2).

Para ello debemos realizar lo siguiente:

-Comentar la línea bind-address del fichero de configuración mysql (maestro).

![comentar la linea bind-address](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/comentar%20la%20linea%20bind-address.PNG)

-Añadir el fichero log_error para que nos avise de los errore, establecer el id del servidor y, por último el log_bin para transacciones seguras.

![añadir fichero log_error](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/a%C3%B1adir%20el%20fichero%20log_error.PNG)

-Reiniciar el servicio mysql.

![reiniciamos el servicio mysql](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/reiniciamos%20el%20servicio%20mysql.PNG)

-Hacemos la misma configuración en el esclavo, con la única diferencia de que el server-iddebe ser 2 en este caso:

![configuración esclvo server id 2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/configuracion%20esclavo%20server%20id%202.PNG)

![restart sql esclavo](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/restart%20sql%20esclavo.PNG)

-Creamos un usuario en el maestro y le damos permiso para la replicación. Dicho usuario será esclavoy su contraseña esclavotambién.

![creamos un usuario en el maestro](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%203/imagenes/activeystatusngingx.png)

-Indicamos al esclavo los datos del maestro y lo iniciamos.

![indicamos al esclavo los datos y lo iniciamos](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/indicamos%20al%20esclavo%20los%20datos%20y%20lo%20inicamos.PNG)

-Volvemos al maestro y volvemos a activar las tablas para poder introducir nuevos datos.

![desbloqueamos tablas](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/desbloqueamos%20tablas.PNG)

-Por último ejecutamos el comando SHOW SLAVE STATUS\G y comprobamos que la variable Seconds_Behind_Master.Sin embargo, realizando esta práctica me encontré que al llegar aquí, tenía este fallo:

![show slave status](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/SHOW%20SLAVE%20STATUS.PNG)

El fallo de la imagen anterior es debido a que cuando cree las máquinas la cloné para seguir con el resto de prácticas y tiene el mismo id en mysql he encontrado aquí la solución al problema.

![resolver error status](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/resolver%20error%20status.PNG)

-Pero a continuación me encontré con el siguiente error en el apartado de Seconds_Behind_Master:
![error 1236](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%205/imagenes/error1236.PNG)

"No se pudo encontrar el nombre del primer archivo de registro en el archivo de índice de registro binario". Sin embargo he repetido la configuración comprobando todos los nombres pero todos son similares y no he conseguido encontrar solución antes de la entrega establecida.

