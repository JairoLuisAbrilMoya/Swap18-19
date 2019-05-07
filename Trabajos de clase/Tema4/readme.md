
### Tema 4: Balanceo de carga

#### 6. Buscar información sobre los bloques de IP para los distintos países o continentes. Implementar en JavaScript o PHP la detección de la zona desde donde se conecta un usuario

Podemos encontrar información sobre bloques de IP en la siguiente página: NirSoft

Para implementar en Javascript la detección de la zona a la que pertenece la IP es necesario recurrir a un servicio externo. Podemos encontrar ejemplos de código de cómo implementarlo usando diferentes servicios: StackOverflow

En el caso de PHP, podríamos usar el paquete GeoIP: GeoIP

#### 7. Buscar información sobre métodos y herramientas para implementar GSLB.

Una herramienta para implementar GSLB es Amazon Route 53 que está dentro de los servicios de Amazon Web Services, que nos permite realizar un GSLB mediante un sistema global de servidores DNS con Elastic Load Balancing.

Otra de las posibles formas de crear un GSLB es usar Nginx y Nginx Plus. Podemos ver como podemos usar Nginx Plus y Amazon Route 53 para hacer un GSLB, junto con la configuración tanto en Amazon como en Nginx, en la siguiente página: GSLB


