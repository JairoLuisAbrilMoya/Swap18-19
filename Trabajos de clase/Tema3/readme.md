### Tema3
### Ejercicio 3.1 Buscar con qué órdenes de terminal o herramientas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra.

* En windows: Para windows server 2008 tendremos que agegar la funcion de "servicios de acceso y directiva de redes" y añadir "servicios de enrutamiento y acceso remoto", en windows todo se puede hacer desde el entorno gráfico, solo hay que darle a siguiente e instalar. Una vez instalada la herramienta, solo tenemos que pinchar encima de "enrutamiento y acceso remoto" y darle a configurar y habilitar, aquí habilitaremos la opción de "traducción de direcciones de red NAT", seleccionamos la interfaz de red que hará de puerta de enlace y configuraremos la ip, la mascara de subred, la puerta de enlace y los DNS. En el resto de equipos tendremos que poner como puerta de enlace la ip de nuestro windows server para que sea el que haga el NAT.
    
* En Linux: En linux, el enrutamiento se puede hacer con iptables. Lo vamos a ver con un ejemplo, en el que tenemos un servidor que une la intranet con una red interna y queremos montar un NAT. Las tarjetas de red tienen del servidor tienen las IPs 10.25.25.2 la que conecta con la intranet y 172.16.20.2 la que conecta con la red interna.

Primero tenemos que permitir el IP Forwarding, para que pasen paquetes de un interfaz a otro.

echo 1 > /proc/sys/net/ipv4/ip_forward

Para que se aplique cada vez que reiniciemos tenemos que añadirlo al fichero /etc/sysctl.conf esta linea

net.ipv4.ip_forward = 1

A continuación tendriamos que habilitar el NAT para que los nodos de la red oculta puedan acceder a los nodos de la intranet.

/sbin/iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

Tras ejecutarlo, ya todos los nodos de la red oculta podrán acceder a nodos de fuera. Para permitir la comunicación en el sentido inverso, desde la intranet a la red oculta, tendríamos que hacer lo equivalente a cuando “abrimos puertos”

/sbin/iptables -A PREROUTING -t nat -p tcp -d 10.25.25.2 --dport 224 -m state --state NEW,ESTABLISHED,RELATED -j DNAT --to 172.16.20.4:22

Hacemos lo mismo con conexiones que lleguen a la IP 10.25.25.2 y puerto 225, que redirigimos “NATeadas” a la IP 172.16.20.5, puerto 22 (SSH):

/sbin/iptables -A PREROUTING -t nat -p tcp -d 10.25.25.2 --dport 225 -m state --state NEW,ESTABLISHED,RELATED -j DNAT --to 172.16.20.5:22

#### Ejercicio 3.2 Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes.

* En linux: se usa iptables, no pongo ejemplos porque me remito a la práctica de este tema donde usamos iptables para bloquear o permitir paquetes desde ips concretas o de algun protocolo concreto o bloquear todo el tráfico etc.
* En windows: Se puede usar el firewall de windows. En esta web podemos ver las opciones que hay de filtrado.


