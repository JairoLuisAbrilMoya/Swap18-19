## Exportación de máquinas virtuales a otro hardware anfitrión
### 1. Introducción

El tema a tratar será, como se indica en el título, como una máquina virtual común puede "clonarse" en otra máquina anfitrión con un hardware distinto al hardware anfitrión de la máquina original. Open Virtualization Format (OVF) y Open Virtualization Appliance (OVA) son las dos presentaciones del estándar que nos permite importar máquinas virtuales alojadas en un determinado hardware anfitrión para transferir la información de dicha máquina virtual a otro hardware distinto, para exportar los archivos creados mediante el estándar (ya sean .ovf o .ova) en el nuevo hardware en el que queremos clonar la máquina virtual. Todo este proceso de clonado puede ser realizado con la ayuda de los medios software de virtualización, como pueden ser VirtualBox o VMware
 
### 2. ¿Qué es la DMTF?

Distributed Management Task Force (DMTF) es una organización dedicada al desarrollo de estándares para la gestión de sistemas en entornos de Tecnologías de la Información empresariales. Buscando la construcción de componentes de infraestructura independientes de la plataforma, facilitando así a la interoperabilidad de sistemas de gestión entre productos de diferentes fabricantes o empresas.
Esta organización ha contado con la participación de empresas como AMD, Cisco, HP, Huawei, IBM, Intel, Microsoft, Oracle, VMware, etc, llegando a tener en 2012 más de 4000 participantes activos dentro de las más de 200 organizaciones y empresas adscritos a la DMTF.
 Además esta organización cuenta con una serie de procesos para el diseño como es el caso del proceso de incubación que está diseñado para fomentar y acelerar el trabajo técnico abierto, colaborativo y exploratorio que complementa la misión del DMTF para liderar el desarrollo, la adopción y la promoción de iniciativas y estándares de gestión interoperables.
 
 
**Entre los estándares de DMTF se incluyen:**
1. Common Information Model (CIM)  – El esquema CIM es un esquema conceptual que define cómo los elementos gestionados en un entorno de TI (por ejemplo, ordenadores o redes de área de almacenamiento) se representan como un conjunto común de objetos y relaciones entre ellos. CIM es extensible para permitir ampliaciones específicas de productos para la definición común de estos elementos gestionados. CIM utiliza un modelo basado en UML para definir el esquema CIM. CIM es la base para la mayoría de los otros estándares DMTF.

2. Common Diagnostic Model (CDM) – El esquema CDM es una parte del esquema CIM que define cómo los diagnósticos del sistema deben ser incorporados en la infraestructura de gestión.

3. Web-Based Enterprise Management (WBEM) – define protocolos para la interacción entre componentes de infraestructura de gestión de sistemas que implementan CIM, un concepto de perfiles de gestión de DMTF, que permite definir el comportamiento de los elementos definidos en el esquema CIM, el CIM Query Language (CQL) y otras especificaciones necesarias para la interoperabilidad de la infraestructura CIM.

4. Systems Management Architecture for Server Hardware (SMASH) – es una iniciativa de gestión de DMTF que incluye perfiles de gestión para la gestión de hardware de servidor. SMASH 2.0 permite WS-Management o SM-CLP (un protocolo de línea de comandos para interactuar con la infraestructura CIM). SM-CLP fue adoptado como un estándar internacional en agosto de 2011 por el Comité Técnico Mixto 1 (JTC 1) de la Organización Internacional de Normalización (ISO) y la Comisión Electrotécnica Internacional (IEC).

5. System Management BIOS (SMBIOS) – define cómo la interfaz de la BIOS de los sistemas de arquitectura x86 es representada en CIM (y DMI).

6. Alert Standard Format (ASF) – define el control remoto y las interfaces de alerta para entornos carentes de sistema operativo (por ejemplo, un controlador de la placa base de un PC).

7. Directory Enabled Network (DEN) – define cómo los directorios LDAP pueden ser usados para proporcionar acceso a los elementos gestionados por CIM y define las asignaciones de CIM a LDAP para una parte del esquema CIM.

8. Desktop Management Interface (DMI) – DMI fue el primer estándar de gestión de escritorios. Debido al rápido avance de las tecnologías de DMTF, tales como CIM, la DMTF definió un proceso de "fin de vida" para DMI, que terminó el 31 de marzo de 2005.

9. Desktop and mobile Architecture for System Hardware (DASH) – un estándar de gestión basado en DMTF Web Services for Management (WS-Management), para sistemas de escritorio y de cliente móvil.

10. Configuration Management Database Federation (CMDBf) - CMDBf facilita el intercambio de información entre bases de datos de gestión de configuraciones (CMDBs) y otros repositorios de datos de gestión (MDRs). El estándar CMDBf permite a las organizaciones federar y acceder a información de infraestructuras complejas y de múltiples proveedores, simplificando el proceso de gestión de datos relacionados con la configuración almacenados en múltiples CMDBs y MDRs.

11. Virtualization Management Initiative (VMAN) – Un conjunto de especificaciones basadas en CIM de DMTF que ayuda a los administradores de TI a: desplegar sistemas de ordenadores virtuales, descubrir/inventario de sistemas de ordenadores virtuales, gestionar el ciclo de vida de los sistemas de ordenadores virtuales, crear/modificar/borrar recursos virtuales y monitorizar sistemas virtuales para salud y rendimiento. VMAN fue adoptado como estándar nacional por el International Committee for Information Technology Standards (INCITS) del American National Standards Institute (ANSI) [2] en junio de 2012.

**Dentro de la iniciativa VMAN, hay varias especificaciones y perfiles:**

1. Open Virtualization Format (OVF) – Estándar para el empaquetado y despliegue de dispositivos virtuales. OVF fue adoptado por el International Committee for Information Technology Standards en agosto de 2010. OVF fue adoptado como un estándar internacional en agosto de 2011 por el Comité Técnico Mixto 1 (JTC 1) de la Organización Internacional de Normalización (ISO) y la Comisión Electrotécnica Internacional (IEC) 
Este es uno de los dos estándares en los cuales nos centraremos para el estudio del tema propuesto.

2. DSP1042 – System Virtualization Profile

3. DSP1057 – Virtual System Profile

4. DSP1059 – Generic Device Resource Virtualization Profile

5. DSP1041 (enlace roto disponible en Internet Archive; véase el historial y la última versión). – Resource Allocation Profile

6. DSP1043 – Allocation Capabilities Profile

### 3. ¿Qué es OVF/OVA?

Como se ha comentado en la introducción, OVF y OVA son el estándar que nos permite importar máquinas virtuales de un hardware a otro. Este fue desarrollado bajo el nombre de Open Virtual Machine Format en septiembre de 2007 por la DMTF. El formato de virtualización abierto ( OVF ) es un estándar abierto para empaquetar y distribuir dispositivos virtuales o, más generalmente, software para ejecutar en máquinas virtuales .
Su finalidad es la de describir un formato abierto, seguro, portable, eficiente y extensible para empaquetación y distribución de software a ejecutar en máquinas virtuales.
El estándar OVF no está vinculado a ningún hipervisor o arquitectura de conjunto de instrucciones en particular . La unidad de empaquetado y distribución es un denominado paquete OVF que puede contener uno o más sistemas virtuales, cada uno de los cuales puede implementarse en una máquina virtual.
### 4. ¿Qué diferencias hay entre el formato OVF y el formato OVA?

Un OVF se compone generalmente de varios archivos. 
Por lo general, este es un pequeño archivo de descripción (.ovf), los archivos de estado de las máquinas virtuales (es decir, su disco - * .vhd o * .vmdk) y, a veces, un archivo de manifiesto (.mf): es un estándar abierto, por lo que no se limita a VMware. 
El ejemplo clásico es si descarga el dispositivo vCenter Server 5.1, es un pequeño archivo .ovf (9.1KB) y 2 discos (disco del sistema - 2GB, disco de datos - 8.4MB).
Un OVA es un archivo único que consiste en un archivo TAR de todos los archivos OVF anteriores (para VMware, esto es usualmente los archivos .ovf y .vmdk). 
Supongo que puede decir que el propósito del OVA es facilitar la vida al empaquetar un dispositivo para la portabilidad y la instalación.
Como ya se ha adelantado anteriormente en la introducción, ambos formatos son dos presentaciones de un mismo estándar realmente.

La diferencia que existe entre estas dos es que en la presentación OVF, el empaquetado se presenta en distintos archivos, que normalmente son un archivo .ovf, que contiene la definición de la máquina virtual, uno o varios archivos .vmdk/.vhd/.vdi que son los ficheros correspondientes a los discos virtuales, y un archivo .mf (manifest file).

En cambio, en la presentación OVA, todo se empaqueta en un mismo archivo con extensión .ova, que, en resumen, empaqueta mediante TAR todos los archivos descritos en la presentación OVF en un solo archivo. Esto es una ventaja pues unifica dichos archivos en uno solo, lo cual hace más sencillo su manejo.

![4.1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/4.1.jpg)

### 5. ¿Qué es una Virtual Appliance?

Una Virtual Appliance (VA) es una máquina virtual preconfigurada y lista para ser utilizada en un hipervisor (medio software para poner en funcionamiento las máquinas virtuales).
Por lo general, son máquinas configuradas con un determinado Sistema Operativo y con unas determinadas aplicaciones preinstaladas para cumplir con algún propósito específico. Son el mejor ejemplo de importación de máquinas virtuales preparadas para poder ser utilizadas y funcionar en cualquier tipo de hardware.
La implementación de una aplicación como un dispositivo virtual puede eliminar problemas con la instalación y configuración, como problemas de compatibilidad de software o controladores. Los usuarios pueden simplemente descargar un solo archivo y ejecutar la aplicación. Los recursos requeridos para el mantenimiento también se reducen. Los dispositivos virtuales han demostrado ser útiles en la implementación de aplicaciones de red. También son útiles en la computación grid, donde pueden resolver los problemas introducidos por hardware y sistemas operativos heterogéneos, y en el modelo de entrega de Software como servicio ( SaaS ), donde la simplicidad del dispositivo virtual puede ayudar a mejorar las economías de escala.
Existen dos tipos de dispositivos virtuales, cerrados y abiertos. Un VA cerrado siempre se empaqueta, distribuye, mantiene, actualiza y administra como una unidad. Una VA abierta es accesible a los clientes para modificaciones. Los desarrolladores pueden incluir una interfaz web para configuraciones personalizadas o entregar parches y actualizaciones.
 
### 6.Ejemplo de importación de Máquina Virtual en nuestro hardware anfitrión.

Nos dirigimos por ejemplo a la página descargarmaquinasvirtuales.com, que nos ofrece ya máquinas virtuales preconfiguradas.

Allí seleccionamos la distribución que deseemos y descargamos los archivos necesarios para la importación de la máquina virtual.Una vez descargado el archivo .ova, procedemos a importarlo.

En nuestro caso hemos decidido descargarnos un sistema operativo fedora como podemos ver a continuación:

![6.2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/6.2.PNG)

Al realizar la importación podremos observar en la siguiente imagen, la máquina virtual funciona con normalidad una vez iniciada:

![6.3](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/6.3.PNG)

### 7.Alternativas a la hora de importar una máquina virtual

Hay otra forma de importar máquinas virtuales, importando el disco duro virtual de la máquina. La extensión de los archivos que guardan la información relativa a los discos duros virtuales suelen ser .vmdk/.vhd/.vdi,, como hemos visto, estos también se importan junto con los archivos .ovf y .mf cuando hacemos uso del estándar OVF.

La importación de únicamente el disco duro nos importará el software contenido en el mismo, pero no la configuración de la máquina virtual en cuestiones hardware específicas (como la RAM con la que haya sido configurada inicialmente, o las tarjetas de red necesarias para el correcto funcionamiento de la misma) dado que no se importan los datos asociados al archivo .ovf contenidos en una importación completa, aunque esto, en configuraciones sencillas no nos supondrá problema alguno.

Nos dirigimos a una página web que nos aporte discos duros virtuales con distribuciones ya preinstaladas y descargamos un disco duro virtual:

![7.1](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/7.1.jpg)

Una vez descargado el archivo .ova, procedemos a crear una máquina virtual nueva e importamos el disco duro virtual descargado:

![7.2](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/7.2.PNG)

Como se puede ver en la siguiente captura, el software de la máquina ha sido importado y tal y como nos indicaba la página de descarga, el disco duro ya tiene instalada la distribución elegida:

![7.3](https://github.com/JairoLuisAbrilMoya/Swap18-19/blob/master/Trabajo%20Final/Imagenes/7.3.PNG)

### 8.Bibliografía

    * https://web.archive.org/web/20090430102409/http://www.dmtf.org/about/clo 

    * https://en.wikipedia.org/wiki/Open_Virtualization_Format

    * https://es.wikipedia.org/wiki/Distributed_Management_Task_Force

    * https://thevirtualunknown.co.uk/2013/03/07/what-is-the-difference-between-ova-and-ovf/

    * https://www.sysadmit.com/2013/12/vmware-formatos-ova-y-ovf.html

    * https://en.wikipedia.org/wiki/Virtual_appliance

    * https://www.redeszone.net/2017/11/11/descarga-maquinas-virtuales-sistema-linux-virtualbox-vmware-lista-funcionar/

    * https://descargarmaquinasvirtuales.com/

    * https://virtualboxes.org/images/

    * http://drivemeca.blogspot.com.es/2013/03/importando-una-maquina-virtual-en.html


