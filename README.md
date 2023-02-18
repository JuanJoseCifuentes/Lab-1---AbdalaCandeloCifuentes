# Laboratorio #1 - Administración de Redes 
## Juan José Cifuentes Cuellar
## Tomas Candelo Montoya
## Carlos Farouk Abdalá Rincón 
> ```
Wiki documentación elaborada para el primer laboratorio de la materia de Administración de Redes

<<<<<<< HEAD
# Planificación
## Preguntas
**¿Cuántas subredes se necesitan?**     
En total necesitamos 5 subredes: La intranet de Bogotá (que cuenta con la vlan 50, 100 y 99 nombradas ing, tech y nativa respectivamente), la intranet de Madrid (que cuenta con la vlan 25, 100 y 99 nombradas Tesorería, Vice y nativa respectivamente), el espacio DMZ, la conexión entre el router de Bogotá y el router ISP correspondiente, la conexión entre el router de Madrid y el router ISP que le corresponde y el espacio de redes WAN

**¿Cuántos host/interfaces necesita la subred, y que dispositivos se encuentran en ella?**  
•	La intranet de Bogotá 172.17.0.0/16 para un total de 950 para cada una de las vlans de esta (50,100,99), en esta se encuentran 4 PC’s, 3 switches, 1 router y un servidor DHCP  
•	La intranet de Madrid 10.111.0.0/16 para un total de 450 host por cada una de sus vlan (25,100,2), en esta se encuentran 4 PC’s, 32switches, 1 Multilayer switch y 1 router     
•	Para el DMZ y el ISP_BOG 161.125.6.0/24 para un total de 6 host para el apartado DMZ, en el cual se encuentran 2 servidores (uno DMZ y uno de web_service) y 1 switch. Y 2 host para la ISP_BOG (conectividad entre el r1_BOG e ISP_BOG), en este se encuentran 2 routers       
•	Para la ISP_ESP 216.169.1.0 para un total de solo 2 host (conectividad entre el R2_ESP e ISP_ESP) donde solo se encuentran los 2 routers mencionados        
•	Por último, es el espacio de redes WAN la cual de igual manera necesitan 2 host por cada conexión con la que cuentan (un total de 5 conexiones, espacios de red), donde se encuentra un total de 4 routers 

**¿Qué partes de la red utilizan direcciones publicas y que partes direcciones privadas?**      
EN la topología las zonas que cuentan con direcciones IP privadas son las dos intranets presentes (Bogotá y Madrid) y las direcciones públicas están en la parte de internet o conexión WAN y en el DMZ donde están los servidores

**¿Dónde deberían estar conservadas estas direcciones?**        
Las direcciones están conservadas en todos los dispositivos presentes de la topología a excepción de los PC’s donde no es necesario que la tengan, pero si es necesario que estén conectados a la vlan que le corresponde según la guía del laboratorio.        

Los routers generarían problemas en sus puertas de enlace si no las tuviese alojadas, , los servidores las conservan para mantener sus servicios y los switches lo tienen para no afectar el direccionamiento de las vlans troncales de la intranet (99 para la intranet de Bogotá y 2 para la intranet de España)

**¿Se requiere una asignación dinámica y/o estática? ¿Donde?**

Si, se necesitan los dos tipo de asignaciones, para las asignaciones estáticas estas se realizaron en los dispositivos que pertenecen a el DMZ, el espacio WAN y los routers de cada una de las dos intranet, estos se han de manera estática tanto por su naturaleza como por la necesidad de configuración que se tiene que satisfacer, en cuanto a la asignación dinámica se configuro un servidor con servicio DHCP para la intranet de Bogotá que permitirá la asignación dinámica para los PC’s y configuramos este mismo servicio en el R2_ESP para así hacer una asignación dinámica a la intranet de Madrid. 

**¿Traducción de direcciones de forma dinámica y/o estática y/o por puertos?**

El NAT se hace de manera dinámica, es decir que permite que cualquier dirección dentro de un rango dado se traduce a una dirección que solicite

**¿En que terminal se configuraron los servicios requeridos?**

Como ya fue mencionado el servicio DHCP se configuró en dos terminales diferentes, en su respectivo servidor en la intranet de Bogotá y en el router R2_ESP para la intranet de Madrid, otro servicio que se utilizo fue el DNS el cual se configuro dentro del servidor alojado en el DMZ al igual que le servidor web con su debido HTTP y HTTPS. Otro servicio utilizado fue el Access list para limitar el acceso al servidor web en las diferentes vlans según las especificaciones dadas en el documento del laboratorio. Por último el servicio NAT se necesita para hacer la traducción de las direcciones privadas, de las intranets, a direcciones públicas para poder acceder a la región del internet, este servicio esta configurado en cada uno de los routers que se conectan a las intranets (R1_BOG y R2_ESP)

**¿Qué servicio de IPv4 se debe configurar para para limitar el acceso al servidor web por el puerto 80 y 443 en las vlans especificas?**

Se necesita utilizar un acceso list que bloquee la puerta de enlace que se pide para la vlan que se pide, en este caso las condiciones que tenemos son las siguientes: 

•	La intranet de Bogotá no tiene el acceso a la página por medio del servicio HTTP (puerto 80) solo tiene acceso por HTTPS (puerto 443), a excepción de la vlan de tecnología que puede acceder por ambos puertos

•	En la intranet de Madrid la vlan de vicepresidencia no cuenta con acceso a ninguno de los dos servicios, tienen totalmente bloqueado el acceso a la pagina de internet alojada en el web Server

•	Mientras que en la misma intranet la vlan de Tesorería solo pueden acceder a través del servicio HTTP (puerto 443) mas no puede usar HTTP (puerto 80)  

Estos Access list se encuentra en los routers que conectan con cada una de las intranets (R1_BOG R2_ESP) con la configuración presentada en este mismo documento.

**¿En que interfaces se deben configurar las OSPF o EIGRP (no RIP) y/o rutas estáticas?**

Para el caso de este laboratorio se utilizo protocolo EIGRP como protocolo de enrutamiento dinámico para el campo de redes WAN, las interfaces que tienen configurados los router del internet son las direcciones de identificación de los puertos vecinos, para aprender las diferentes rutas con las que cuenta, la configuración de igual manera esta presente en este documento en el apartado de configuración de la topología

## Subneteo

La red empresarial cuenta con un total de 5 procedimientos de Subneteo

•	La intranet de Bogotá 172.17.0.0/16 para un total de 950 para cada una de las vlans de esta (50,100,99)

•	La intranet de Madrid 10.111.0.0/16 para un total de 450 host por cada una de sus vlan (25,100,2)

•	Para el DMZ y el ISP_BOG 161.125.6.0/24 para un total de 6 host para el apartado DMZ y 2 host para la ISP_BOG (conectividad entre el r1_BOG e ISP_BOG)

•	Para la ISP_ESP 216.169.1.0 para un total de solo 2 host (conectividad entre el R2_ESP e ISP_ESP)

•	Por último, es el espacio de redes WAN la cual de igual manera necesitan 2 host por cada conexión con la que cuentan (un total de 5 conexiones, espacios de red)

Para el proceso de Subneteo se tubo en cuenta la cantidad de host necesarios para cada espacio de direcciones y la cantidad de rengos de red que se necesitaran dependiente de las vlans o las conexiones.


**Subneteo de la intranet de Bogotá**

![SUBNETEO DE LA INTRANET DE BOGOTÁ](/img/subneteo_Bog.jpg.png)

Para este Subneteo el numero de host pedidos por las especificaciones de la red es de 959 lo que da un total de 10 bits de reserva, lo que da una nueva mascara de sub red con identificador 22 y un incremento de 4 en el tercer octeto de la dirección, como se puede ver en la imagen se usaron 3 rangos de direcciones para las 3 vlans solicitadas (50 ing, 100 tech y la 99 nativa).

**Subneteo de la intranet de Madrid**

![SUBNETEO DE LA INTRANET DE MADRID](/img/SUBNETEO_MAD.png)

Para este Subneteo el número de host pedidos por las especificaciones de la red es de 450 lo que da un total de 9 bits de reserva, lo que da una nueva mascara de subred con identificador 23 y un incremento de 2 en el tercer octeto de la dirección, como se puede ver en la imagen se usaron  de igual manera 3 rangos de direcciones para las 3 vlans solicitadas (25 Tesoreria, 100 Vice y la 2 nativa).

**Subneteo del DMZ**

![SUBNETO DE LA RED DMZ](/img/SUBNETEO_DMZ.png)

Para este Subneteo el número de host pedidos por las especificaciones de la red es de 6 lo que da un total de 3 bits de reserva, lo que da una nueva mascara de subred con identificador 29 y un incremento de 8 en el cuarto octeto de la dirección en este caso solo necesitamos un rango de red como se ve en la imagen

**Subneteo del ISP_BOG**

![SUBNETO DE LA ISP DE BOGOTA](/img/SUBNETEO_ISP_BOG.png)

Para este Subneteo el número de host pedidos por las especificaciones de la red es de 2 lo que da un total de 2 bits de reserva, lo que da una nueva mascara de subred con identificador 30 y un incremento de 4 en el cuarto octeto de la dirección. En este caso solo necesitamos un rango para la conexión entre el R1_BOG e ISP_BOG

**Subneteo del ISP_ESP**

![SUBNETO DE LA RED ISP DE ESPAÑA](/img/SUBNETEO_ISP_ESP.png)

Para este Subneteo el número de host pedidos por las especificaciones de la red es de 2 lo que da un total de 2 bits de reserva, lo que da una nueva mascara de subred con identificador 30 y un incremento de 4 en el cuarto octeto de la dirección. En este caso solo necesitamos un rango para la conexión entre el R2_ESP e ISP_ESP 

**Subneteo del espacio WAN**

![SUBNETO DEL ESPACIO WAN(INTERNET)](/img/SUBNETEO_WAN.png)

Para este Subneteo el número de host pedidos por las especificaciones de la red es de 2 lo que da un total de 2 bits de reserva, lo que da una nueva mascara de subred con identificador 30 y un incremento de 4 en el cuarto octeto de la dirección. En este caso son necesarios 5 rangos de red para las conexiones entre los routers que pertenecen al internet 

## Tabla de direccionamiento

Finalmente con los Subneteos terminados pasamos a la construcción de la tabla de enrutamiento en la cual asignamos las direcciones IP a los diferentes dispositivos de la red y en los puertos que deben de ser asignadas, el resultado de esas asignaciones fue la siguiente tabla:

![Tabla de direccionamiento en la red empresarial](/img/TABLA_DIRECCIONAMIENTO.png)

# Configuarción

## Multilayer switch 

**Requerimientos**
Se necesita utilizar un multilayer switch de numero de referencia 3650

**configuración**
Un multilayer switch es un switch que funciona como un dispositivo de capa 3, para el caso del laboratorio lo utilizaremos como el puente entre la intranet de Madrid con el router de España, la configuración que se utilizo para este se extrajo de un video se YouTube y de diferentes foros . Lo primero es que estuviese configurado todo lo relacionado con los switches y las configuraciones de las vlan en cada uno de ellos, después de eso se realizó la configuración básica que se realiza en todos los dispositivos, la asignación de las contraseñas y el nombre para el multilayer, una vez estas configuraciones completadas, lo primero que se hizo fue crear las vlans dentro del multilayer con los comandos ya aprendidos en proyectos anteriores,
>```
>MLSW(config)#VLAN 25
>
>MLSW(config-vlan)#name Tesoreria
>
>MLSW(config-vlan)#vlan 50
>
>MLSW(config-vlan)#name Vice
>
>MLSW(config-vlan)#vlan 2
>
>MLSW(config-vlan)#name Nativa

luego se configuro el enlace troncal en el rango de interfaces del gigabit Ethernet 1/0/1 y el gigabit Ethernet 1/0/3 en el cual se hizo la configuración de la vlan 2 como la nativa para el procedimiento del truncamiento,
>```
>MLSW(config)#interface range gig 1/0/1-3
>
>MLSW(config-if-range)# switchport mode trunk
>
>MLSW(config-if-range)# switchport trunk native vlan 2

ahora por la interfaz gigabit Ethernet 1/0/1 se le concederá el acceso a esta vlan 2 
>```
>MLSW(config)#interface gig 1/0/1
>
>MLSW(config-if-range)# switchport mode access
>
>MLSW(config-if-range)# switchport access native vlan 2

como se puede ver en la siguiente imagen:
![SHOW RUNNING-CONFIG DEL MULTILAYER SWITCH](/img/CONFIG_MULTILAYER.png)

Después de esto se ingreso a la interfaz de la vlan 2 para hacer la asignación de la dirección ip que le corresponde a nuestro switch la cual hace parte del conjunto de redes para esta vlan (10.111.4.4 con una mascara de res 255.255.254.0) 
>```
>MLSW(config)#interface VLAN 2
>
>MLSW(config-if-range)# IP address 10.111.4.4 255.255.254.0

y por ultimo se le asigno la puerta de acceso 10.111.4.1 
>```
>MLSW(config)#ip default-gateway 10.111.4.1 
=======
# Configuración

## Configuraciones básicas:
Para todos los dispositivos intermedios se hace la misma configuración básica, en la que se le asigna un nombre, un mensaje de bienvenida y contraseñas de acceso tanto para entrar a su configuración (por cable de consola o por telnet) como para acceder a sus diferentes niveles administrativos.

Un ejemplo de estas configuraciones básicas se puede ver a continuación:
```
Switch(config)#hostname SW1_Bog
SW1_Bog(config)#line console 0
SW1_Bog(config-line)#password cisco
SW1_Bog(config-line)#login
SW1_Bog(config-line)#exit
SW1_Bog(config)#
SW1_Bog(config-line)#password cisco
SW1_Bog(config-line)#login
SW1_Bog(config-line)#exit
SW1_Bog(config)#banner motd #Se encuentra configurando el switch 1 en Bogota. Ingrese su contrasena#
SW1_Bog(config)#enable password cisco
```

## Configuraciones para crear VLANs:

**Switches:**

En primer lugar, a cada switch se le asigna una default gateway. En este caso, esta dirección será la dirección de la subinterfaz truncal del router que está conectado a su intranet.
```
SW1_Bog(config)#ip default-gateway 172.17.8.1
```

Ahora, apagamos todas sus interfaces.
```
SW1_Bog(config)#interface range fa0/1-24
SW1_Bog(config-if-range)#shutdown
SW1_Bog(config-if-range)#exit
```

Con las interfaces desactivadas, designamos una serie de interfaces como interfaces troncales, asignandolas a la vlan nativa 99. Las encendemos.
```
SW1_Bog(config)#interface range fa0/1-5
SW1v(config-if-range)#switchport mode trunk
SW1_Bog(config-if-range)#switchport trunk native vlan 99
SW1_Bog(config-if-range)#no shutdown 
SW1_Bog(config-if-range)#exit
```

El siguiente paso es crear todas las VLANs requeridas y nombrarlas.
```
SW1(config)#vlan 50
SW1(config-vlan)#name Ing
SW1(config-vlan)#vlan 100
SW1(config-vlan)#name Tech
SW1(config-vlan)#vlan 99
SW1(config-vlan)#name Native
SW1(config-vlan)#exit
```

Puesto que la guía de laboratorio no especifica que interfaces deben pertenecer a cada VLAN, a continuación asignamos las interfaces restantes de la forma más equitativa posible.
```
SW1_Bog(config)#interface range fa0/6-15
SW1_Bog(config-if-range)#Switchport access vlan 50
SW1_Bog(config-if-range)#exit
SW1_Bog(config)#interface range fa0/16-24
SW1_Bog(config-if-range)#Switchport access vlan 100
SW1_Bog(config-if-range)#exit
SW1_Bog(config)#interface range fa0/1-5
SW1_Bog(config-if-range)#Switchport access vlan 99
SW1_Bog(config-if-range)#exit
```

La última configuración restante es asignar una dirección IP al switch usando su interfaz VLAN 99
```
SW1_Bog(config)#interface vlan 99
SW1_Bog(config-if)#ip address 172.17.8.2 255.255.252.0
SW1_Bog(config-if)#exit
```

Para finalizar, reactivamos todas las interfaces restantes del Switch y guardamos los cambios.
```
SW1_Bog(config)#interface range fa0/6-24
SW1_Bog(config-if-range)#no shutdown
SW1_Bog(config-if-range)#exit
SW1_Bog(config)#exit
SW1_Bog#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

**Routers:**

Para la configuración de las VLAN, el primer paso es encender la interfaz conectada a la intranet.
```
R1_Bog(config)#interface fa0/1
R1_Bog(config-if)#no shutdown
R1_Bog(config-if)#exit
```

A continuación, se crean todas las subinterfaces del router. Vale aclarar que se necesita una subinterfaz por cada VLAN, y que cada una debe ir configurada con encapsulación (Con la subinterfaz 99 especificando que es nativa).

Además, hay que recordar que se utiliza el comando ip helper-address para permitir que los PC reciban su dirección dinámica desde cualquier VLAN.
```
R1_Bog(config)#interface fa0/1.50
R1_Bog(config-if)#encapsulation dot1q 50
R1_Bog(config-if)#ip address 172.17.0.1 255.255.252.0
R1_Bog(config-if)#ip helper-address 172.17.4.2 
R1_Bog(config-if)#interface fa0/1.100
R1_Bog(config-if)#encapsulation dot1q 100
R1_Bog(config-if)#ip address 172.17.4.1 255.255.252.0
R1_Bog(config-if)#ip helper-address 172.17.4.2 
R1_Bog(config-if)#interface fa0/1.99
R1_Bog(config-if)#encapsulation dot1q 99 native
R1_Bog(config-if)#ip address 172.17.8.1 255.255.252.0
R1_Bog(config-if)#ip helper-address 172.17.4.2 
```

Finalmente guardamos los cambios.
```
R1_Bog(config)#exit
R1_Bog#copy running-config startup-config 
Destination filename [startup-config]? 
Building configuration...
[OK]
```

**Router como proveedor DHCP:**

Para la intranet de Madrid, hay que configurar que los PC de las diferentes VLAN puedan recibir una dirección de forma dinámica, y para ello, puesto que no podemos configurar un servidor DHCP, hay que configurar el router con dos diferentes pools de direcciones IP. Para ello, se ejecutan los siguientes comandos:
```
R2_Esp(config)#ip dhcp pool MAD
R2_Esp(dhcp-config)#network 10.111.0.0 255.255.254.0
R2_Esp(dhcp-config)#default-router 10.111.0.1
R2_Esp(dhcp-config)#dns-server 161.125.6.3
R2_Esp(config)#ip dhcp pool MAD100
R2_Esp(dhcp-config)#network 10.111.2.0 255.255.254.0
R2_Esp(dhcp-config)#default-router 10.111.2.1
R2_Esp(dhcp-config)#dns-server 161.125.6.3
```

## Enlaces seriales WAN

Nótese que para la configuración de los enlaces seriales es necesario primero tener planificado las direcciones que serán asignadas a cada interfaz. Con esto claro, es tan fácil como asignar las direcciones a la interfaz correspondiente y asegurarse de encenderla.
```
ISP_NET(config)#interface se0/0/0
ISP_NET(config-if)#ip address 190.85.211.6 255.255.255.252
ISP_NET(config-if)#no shutdown
ISP_NET(config-if)#exit
ISP_NET(config)#interface se0/0/1
ISP_NET(config-if)#ip address 190.85.211.10 255.255.255.252
ISP_NET(config-if)#no shutdown
ISP_NET(config-if)#exit
ISP_NET(config)#interface se0/1/1
ISP_NET(config-if)#ip address 190.85.211.17 255.255.255.252
ISP_NET(config-if)#no shutdown
ISP_NET(config-if)#exit
```
>>>>>>> 78ba47fdfa4f439e0181e8840afdc0dc8836b4b9

## Access Control Lists (ACL's)
**Requerimientos**: El Departamento de Tecnología solicitó los siguientes filtros de paquetes: Todos los hosts de la Intranet Bogotá acceden al servidor Web a través del protocolo HTTPs (puerto 443) y no por HTTP (puerto 80). Los usuarios del área de Vicepresidencia en la Intranet Madrid deben tener restringido el acceso al servidor Web por HTTPs (puerto 443) y HTTP (puerto 80). Los usuarios del Departamento de Tesorería sólo deben acceder al servidor Web por el puerto 443. Finalmente, El Departamento de Tecnología accede al servidor Web por HTTPs (puerto 443) y HTTP (puerto 80). 

**Configuración**: Para esta configuración es necesario establecer las listas del control de acceso (ACL) en los routers R1_BOG y R2_ESP. Lo anterior con el propósito de cumplir con las limitaciones requeridas.

En el caso del R1_BOG será necesario configurar un bloqueo de acceso al servidor web en caso de que se haga uso del servicio HTTP si se pertenece a la Vlan 50 (Ingeniería), es decir, se debe negar el acceso de las direcciones que pertenezcan a dicha Vlan cuando se haga uso del puerto 80. Para esto configuraremos el Access Control List 101 en la interfaz FastEthernet 0/0 de salida, o en otras palabras, para que antes de salir por la interfaz se haga el bloqueo necesario. Se hizo uso de un ACL extendido que nos permitiera colocar dos especificaciones en una sola lista.

```
R1_Bog(config)#access-list (Numero identifiacdor de la ACL) deny tcp (IP de la Vlan) (Subnet Mask o Wild Card) any eq (Puerto)
R1_Bog(config)#access-list (Numero identifiacdor de la ACL) permit ip any any
R1_Bog(config)#interface (Interfaz que se vaya a trabajar)
R1_Bog(config-if)#ip access-group (Numero identifiacdor de la ACL) (out/in)
```

Aplicandolo para el caso del laboratiorio: 

```
R1_Bog(config)#access-list 101 deny tcp 172.17.0.0 0.0.3.255 any eq 80
R1_Bog(config)#access-list 101 permit ip any any
R1_Bog(config)#interface Fa0/0
R1_Bog(config-if)#ip access-group 101 out
```

Lo anterior nos generará el siguiente resultado:

```
R1_Bog# show access-lists
```

![ACCESS-LIST 101](/img/ACCESS-LIST%20101.jpg)

Para el R2_ESP se uso la misma lógica para la especificación del uso de los puertos 443 y 80 en las Vlan’s 25 (Tesorería) y 100 (Vice), pues la 25 solo podía hacer uso de el servicio HTTPS, pero, a diferencia que, en el caso anterior, la Vlan 100 no podría hacer uso de ningún servicio del servidor Web, por ello se decidió bloquear cualquier tipo de interacción de cualquier dirección perteneciente a la Vlan antes mencionada con el servidor. En este caso la interfaz que se usará, también como salida, será la Serial 0/1/1. Cabe recalcar que la dirección del servidor Web es 161.125.6.2

```
R2_Esp(config)#access-list 102 deny tcp 110.111.0.0 0.0.1.255 any eq 80
R2_Esp(config)#access-list 102 deny ip 110.111.2.0 0.0.1.255 host 161 125.6.2
R2_Esp(config)#access-list 102 permit ip any any
R2_Esp(config)#interface Serial0/1/1
R2_Esp(config-if)#ip access-group 102 out
```

```
R2_Esp# show access-lists
```
![ACCESS-LIST 102](/img/ACCESS-LIST%20102.jpg)

Para ambos casos se añadió la especificación “permit ip any any” con el propósito de confirmar que cualquier interacción además diferente a las limitadas en los anteriores enunciados, fueran permitidas.

A continuación, una comprobación de la funcionalidad de las Access Control List:

![R1_BOG_PC1_ACCESS-LIST](/img/R1_BOG_PC1_ACCESS-LIST.jpg)
![R1_BOG_PC2_ACCESS-LIST](/img/R1_BOG_PC2_ACCESS-LIST.jpg)
![R2_ESP_PC5_ACCESS-LIST](/img/R2_ESP_PC5_ACCESS-LIST.jpg)
![R2_ESP_PC6_ACCESS-LIST](/img/R2_ESP_PC6_ACCESS-LIST.jpg)

## Enrutamiento EIGRP (Enhanced Interior Gateway Routing Protocol)

Para el enrutamiento de este laboratorio hemos elegido el protocolo EIGRP principalmente, por que es propiedad de cisco lo que nos puede dar la idea de que es más sencillo para manejar en el programa, y segundo por que EIGRP maneja una menor distancia administrativa a comparación de otros protocolos.

Para configurar el enrutamiento es necesario entrar a la configuración de cada uno de los routers, y en su configuración de terminal colocar el comando:

```
R2_Esp(config)#router EIGRP (Número de Identificación del protocolo)
```

Esto nos dará entrada a la configuración del protocolo en la que deberemos especificar las direcciones de identificación de todas las redes a las que el router este directamente conectado de la siguiente manera: 

```
R2_Esp(config-router)#newtork (IP identificadora de la red)(Wild Card)
```

Para el caso del laboratorio, la configuración fue la siguiente: 

```
R2_Esp(config)#router EIGRP 1
R2_Esp(config-router)#newtork 216.169.1.2 0.0.0.3
R2_Esp(config-router)#network 10.111.0.0 0.0.255.255
```

Después de hacer esto en cada uno de los routers con sus respectivas redes vecinas, el protocolo se encargará de hacer que cada uno de los routers aprendan las redes conectadas a cada uno de sus vecinos, completando sus tablas de enrutamiento para cada uno de los destinos necesarios.

A continuación, se puede ver como quedaría la guía de enrutamiento de uno de los routers, específicamente el R2_ESP:

![SHOW_IP_ROUTE-R2_ESP](/img/SHOW%20IP%20ROUTE%20-%20R2_ESP.jpg)

Podemos ver como la gran mayoría empieza con la letra D, lo que según las instrucciones del comando “show ip route” se refiere a las direcciones de redes aprendidas por medio del protocolo EIGRP, o en otras palabras, las redes que los otros routers le proporcionaron al router en el que nos encontramos, acompañadas de la dirección de la interfaz del dispositivo que las proporciono y por cual interfaz se recibió la información en este caso podemos ver que solo aparece la dirección del Serial 0/1/1 del router ISP_ESP porque es la única conexión que tiene el router R2_ESP con la red de routers. Además, podemos ver las direcciones de las redes que están directamente conectadas a el router.

## Servidores

La configuración de los diferentes servidores es muy simple, puesto que solo hay que configurar el servicio deseado desde la pestaña de servicios.

**Servidor DHCP:**
Se le asigna una dirección estática dentro de la VLAN 100 (requerimiento) y luego se configuran dos pools de direcciones IP, con la predeterminada "serverPool" sirviendo como la pool de la VLAN en la que se encuentra.

![Configuracion del server DHCP](/img/dhcp.png)

**Servidor DNS:**
Después de asignarle una dirección estática, es tan simple como ir a la pestaña de servicios y asignar el nombre requerido (iniciales_integrantes.net) junto con la variación www a la dirección que tiene el servido Web.

![Configuracion del server DHCP](/img/dns.png)

**Servidor Web:**
Se le asigna una dirección estática dentro de la VLAN 100 (requerimiento) y luego se configuran dos pools de direcciones IP, con la predeterminada "serverPool" sirviendo como la pool de la VLAN en la que se encuentra.

![Configuracion del server DHCP](/img/web.png)

# Verificación
## PDU's

Para esta parte consideramos analizar el flujo bidireccional de un mensaje del PC1 al servidor por medio del comando telnet que nos permite especificar por que puerto salir, esto para poder ver de manera detallada como se comporta el mensaje dependiendo el puerto con por el que salga. Primero haremos la prueba con el puerto 80.

>```
>>telnet (Dirección con la que se quiera probar conexión) (Puerto)

>```
>>telnet 161.125.6.2 80

![PDU_LIST-PC1-PORT80](/img/PDU_LIST-PC1-PORT80.jpg)

Como era de esperar, nos arrojo error, pues el puerto 80 esta programado para no permitir el acceso o el paso de paquetes provenientes de la Vlan de Ingeniería a la cual pertenece el PC1. Podemos ver que la primera parte de los eventos se refieren a como llega el telnet del PC1 al R1_BOG, por lo que solo nos centraremos en donde sale el error.

![PDU_DETAILS-PC1-PORT80](/img/PDU_DETAILS-PC1-PORT80.jpg)

Podemos ver que el router devolverá un mensaje en el que se manifiesta que el mensaje esta administrativamente bloqueado, pues la ACL creada anteriormente bloquea el paso de los mensajes de la Vlan con identificador 172.17.0.0 a la cual pertenec el PC desde l cual se está haciendo el Telnet.

A continuación, el mensaje se devolverá por la misma ruta por la que llego hasta el PC1 hasta llegar a su destino y manifestar que no se pudo completar la acción como se vio en la lista de eventos.

Este mismo proceso se repetirá varias veces hasta que el dispositivo determine que la ruta es inalcanzable, en este caso se mandaron 12 mensajes, todos con las mismas características y rechazados en el R1_BOG.
Para el caso del puerto 443:

>```
>>telnet 161.125.6.2 443

![PDU_LIST-PC1-PORT443](/img/PDU_LIST-PC1-PORT443.jpg)

Se puede ver que el mensaje fue mandado y recibido de manera correcta, por lo que se logro establecer conexión con el servidor satisfactoriamente. En el mismo punto en el que falló con anterioridad al usar el puerto 80, podemos ver las siguientes acciones:

![PDU_DETAILS-PC1-PORT443](/img/PDU_DETAILS-PC1-PORT443.jpg)
![PDU_DETAILS-PC1-PORT443](/img/PDU_DETAILS-PC1-PORT443_2.jpg)

Podemos ver que esta vez nos manifiesta que el puerto tiene acceso y por ello deja se deja pasar la información hasta el servidor quien posteriormente reenviara un mensaje para confirmar la conexión.

# Finalización
## Retos

Después de haber realizado el laboratorio, podemos determinar que quizá los mayores retos que manejamos fue a la hora de desarrollar el enrutamiento y las Access Control List, pues por diversas y lamentablemente desconocidas situaciones, a veces estas dos partes tan importantes del proyecto fallaban sin razón aparente. En el caso del enrutamiento, en más de una ocasión, las tablas de enrutamiento se quedaron vacías incluso después de haber comprobado que el protocolo EIGRP había funcionado de manera correcta, lo cual genero dificultades a la hora de probar conectividad en toda la topología. Y en el caso de las Access Control List, consideramos que quizá por errores mínimos en la redacción de las condiciones, se negaba el acceso a toda una intranet sin si quiera mencionarla en las limitantes, por lo que fue necesario redactarlas varias veces. Sin embargo, se supo afrontar estas dificultades y lograr finalizar la práctica de laboratorio.


## Recomendaciones

Después de conocer los retos más destacables a la hora de hacer el laboratorio, consideramos que las mejores recomendaciones serían:

En primer lugar comprobar constantemente el funcionamiento de cada una de las partes del laboratorio para que a la hora de hallar un error, se puedan descartar todas las opciones aun cuando ciertos protocolos ya se encontraban funcionando.

Segundo, tener suprema cautela a la hora de ingresar comandos que contengan direcciones IP como por ejemplo las Access Control List o en la configuración tanto de Switches como Routers, pues un mínimo error al ingresar esta información podría hacer que se generen errores que después no sean tan fáciles de detectar.

Finalmente, creemos que es de suprema importancia manejar el funcionamiento básico de ciertos protocolos y funciones de dispositivos, pues el no comprender lo anterior, conlleva a comenzar a generar errores en cadena que pueden culminar con un desarrollo totalmente inutilizable que complique de más la situación a la hora de la detección de algún error.

Adicionalmente, consideramos que fue de vital ayuda la realización ordenada de todas las tablas de direccionamiento con el propósito de mejorar la eficiencia a la hora de desarrollar el laboratorio.

