# SWAP1819

## Práctica4 : Asegurar la granja web

El objetivo de la práctica 4 es llevar a cabo la configuración de seguridad de la granja web.

### Instalar un certificado SSL autofirmado para configurar el acceso por HTTPS

Para generar nuestro certificado SSL autofirmado debemos hacer lo siguiente:

-Se activan una serie de módulos: a2enmod ssl

-Reiniciamos el servicio de apache2 : service apache2 restart

-Creamos la carpeta donde guardaremos nuestro certificado SSL: mkdir /etc/apache2/ssl

-Generamos el certificado SSL :

`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt`

Como podemos ver nos pide una serie de datos para configurar el dominio.

![ certificado autofirmado](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/instalar%20certificado%20autofirmado.PNG)


A continuación tenemos que editar el archivo de configuración de nuestro servidor para añadir nuestro certificado previamente creado e indicar que lo utilice:

sudo nano /etc/apache2/sites-available/default-ssl

Y añadimos las siguientes líneas:

- SSLCertificateFile /etc/apache2/ssl/apache.crt
- SSLCertificateKeyFile /etc/apache2/ssl/apache.key

![ ssl.conf](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/paso1.2%20a%C3%B1adir%20ss.conf.PNG)

Una vez modificado, tenemos que activar el sitio y reiniciar apache:

![ activar y reiniciar apache](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/activarsitioy%20reiniciar%20apache.PNG)

Para comprobar que funciona hacemos las peticiones mediante curl y como vemos si no ponemos el -k no nos dejaría:

![ comprobacion curl](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/comprobacion%20mediante%20curl.PNG)

Para configurar el servidor web m2 copiaremos los certificados generados en m1 a m2, yo he usado rsync pero tambien se puede hacer con scp.

![ configuracion swap con rasync](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/configurar%20swap2%20a%20traves%20rsync.PNG)

igualmente tenemos que editar el fichero default-ssl y reiniciar el servicio y recargar apache2.

![ modificacion de default-ssl en swap2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/modificacion%20en%20swap%20del%20fichero%20default-ssl.PNG)

también tenemos que copiar los certificados al balanceador, para eso crearemos una carpeta y ejecutaremos el comando rsync

![copiar certificados en el balanceador](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/copiar%20certificados%20en%20el%20balanceador%20nginx.PNG)

Editamos el fichero de configuración de nginx añadiendo los certificados que hemos copiado y que escuche en el puerto 443 ssl.

![ configuracion de nginx](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/ficheroconfiguracion%20modificado%20de%20nginx.PNG)

Reiniciamos el servicio

- sudo service nginx restart

### Configuración del cortafuegos

En primer lugar para ver el estado del cortafuegos ejecutamos: iptables -L -n -v

![ estado cortafuegos](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/estado%20del%20cortafuegos.PNG)

Como vemos se acepta todo el tráfico ya que aún no hay ninguna regla definida.

### IPTABLES

Como vemos iptables tiene muchas funcionalidades para establecer reglas, filtrar tipos de tráfico o bien controlar el acceso a ciertas páginas.
Algunas de ellas pueden ser:

Si queremos que bloquee todo el tráfico tanto de entrada como de salida:

`iptables -P INPUT DROP`
`iptables -P OUTPUT DROP`
`iptables -P FORWARD DROP`

![ bloqueo de entrada y salida ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/bloqueosalida%20y%20entrada.PNG)

Si queremos bloquear el tráfico de entrada y permitir el de salida:

`iptables -P INPUT DROP`
`iptables -P FORWARD DROP`
`iptables -P OUTPUT ACCEPT`
`iptables -A INPUT -m state --state NEW, ESTABLISHED -j ACCEPT`

![ bloqueo de entrada y  permite salida ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/bloqueo%20entrada%20y%20permite%20salida.PNG)

Si queremos bloquear trafico ICMP para evitar ataques mediante ping:

`iptables -A INPUT -p icmp --icmp-type echo-request -j DROP`

![ bloqueo icmp ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/bloqueoicmp.PNG)

Si queremos permitir el acceso por SSH, abriendo el puerto 22:

`iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
`iptables -A OUTPUT -p udp --sport22 -j ACCEPT`

![  permite acceso por ssh ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/permitir%20acceso%20por%20ssh.PNG)

Si queremos permitir el acceso a DNS, abriendo el puerto 53:

`iptables -A INPUT -m state --state NEW -p udp --dport 53 -j ACCEPT`
`iptables -A INPUT -m state --state NEW -p tcp --dport 53 -j ACCEPT`

![   permite acceso a DNS ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/permitir%20acceso%20a%20DNS.PNG)

Si queremos bloquear todo el tráfico de entrada o salida desde una IP:

`iptables -I INPUT -s 192.168.1.100 -j DROP`
`iptables -I OUTPUT -s 192.168.1.100 -j DROP`

![ bloqueo de TRAFICO DE UNA IP ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/bloqueo%20del%20trafico%20de%20una%20ip.PNG)

IDEM para aceptar pero usando ACCEPT en vez de DROP.

Por último para comprobar el funcionamiento del cortafuegos y ver que puertos hay abiertos:

`netstat -tulpn`

![ funcionamiento de cortafuegos ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/ver%20funcionamiento%20de%20cortafuegos.PNG)


### SCRIPT

Lo habitual es crear un script que se ejecute en el arranque del sistema.

Tenemos que crear un script para permitir el acceso por los puertos de HTTP y HTTPS al servidor (máquina1), para ello:

![ Script ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/script.PNG)

Para que se ejecute cuando iniciamos la máquina virtual. Añadimos lo siguiente al crontab:

![ ejecutar al iniciar la mquina ](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Practicas/Practica%204/Imagenes/ejecutarlo%20al%20iniciar%20la%20maquina.PNG)

El parametro & del final sirve para que el proceso se ejecute en segundo plano.











