### Tema 1: Introducción

1. Buscar información sobre las tareas o servicios web para los que se usan más los siguientes programas:

    apache
    nginx
    thttpd
    Cherokee
    node.js

#### Apache

Apache es un servidor HTTP (httpd) para alojar servicios y páginas web desarrollado por Apache Software Foundation a partir de 1995. Debido a su gran popularidad, dispone de una buena documentación, además, es extensible mediante módulos y puede procesar un gran número de lenguajes interpretados sin recurrir a software externo.

#### Nginx

Nginx es un servidor que soporta aplicaciones HTTP, TCP y UDP. Al igual que Apache es también un servidor web. Fue desarrollado para soportar una gran carga de peticiones, algo muy reclamado en la web actual.

#### thttpd

thttpd es un servidor web libre desarrollado por ACME Laboratories, compatible con la mayoría de sistemas Unix, que tiene como principales características:

    * Simple: solo utiliza lo mínimo posible para poder usar el protocolo HTTP.
    * Ligero: el número de archivos que necesita es muy reducido, con tamaños pequeños para estos ficheros y el ejecutable, poco uso de memoria durante la ejecución, etc. Para más información, podemos consultar este enlace.
    * Multiplataforma: como se ha dicho antes, es posible ejecutarlo en una gran variedad de sistemas basados en Unix.
    * Rápido: ante un alto nivel de carga, responde mucho más rápido que otros sistemas.
    * Seguridad: desde ACME Labs., hacen todo lo posible para mantener la seguridad, frente a ataques al servidor, mejorando la seguridad de la herramienta.

#### Cherokee

Es una herramienta que nos permite configurar fácilmente un servidor web, que nos permite instalar una gran cantidad de servicios, poder crear infraestructuras virtuales, soporta muchas herramientas web (FastCGI, SCGI, PHP, uWSGI, SSI, etc.), SSL/TLS, etc. También ofrece una interfaz gráfica para poder configurar el servidor web y poder ejecutar aplicaciones en Django, Ruby On Rails, videostreaming, etc.

#### Node.js

Node.js es un entorno Javascript asíncrono orientado a eventos. Esto quiere decir que el servidor sólo reaccionará cuando un evento ocurra. Está diseñado para construir aplicaciones escalables y con requisitos de tiempo real, puede manejar múltiples conexiones a la vez.

Otros servidores usan hebras del sistema operativo, lo cual es muy ineficiente y difícil de usar. Además, prácticamente ninguna función de Node.js ejecuta I/O por lo que los procesos no se bloquean, esto permite hacer sistemas escalables.

Además, Node.js funciona en Javascript, lo que nos permite usar el mismo lenguaje de programación tanto en el frontend como en el backend.
