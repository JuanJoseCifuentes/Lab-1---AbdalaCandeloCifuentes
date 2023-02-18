# Laboratorio #1 - Administración de Redes 
## Juan José Cifuentes Cuellar
## Tomas Candelo Montoya
## Carlos Farouk Abdalá Rincón 
> ```
Wiki documentación elaborada para el primer laboratorio de la materia de Administración de Redes

# Configuarción
## Access Control Lists (ACL's)
**Requerimientos**: El Departamento de Tecnología solicitó los siguientes filtros de paquetes: Todos los hosts de la Intranet Bogotá acceden al servidor Web a través del protocolo HTTPs (puerto 443) y no por HTTP (puerto 80). Los usuarios del área de Vicepresidencia en la Intranet Madrid deben tener restringido el acceso al servidor Web por HTTPs (puerto 443) y HTTP (puerto 80). Los usuarios del Departamento de Tesorería sólo deben acceder al servidor Web por el puerto 443. Finalmente, El Departamento de Tecnología accede al servidor Web por HTTPs (puerto 443) y HTTP (puerto 80). 

**Configuración**: Para esta configuración es necesario establecer las listas del control de acceso (ACL) en los routers R1_BOG y R2_ESP. Lo anterior con el propósito de cumplir con las limitaciones requeridas.

En el caso del R1_BOG será necesario configurar un bloqueo de acceso al servidor web en caso de que se haga uso del servicio HTTP si se pertenece a la Vlan 50 (Ingeniería), es decir, se debe negar el acceso de las direcciones que pertenezcan a dicha Vlan cuando se haga uso del puerto 80. Para esto configuraremos el Access Control List 101 en la interfaz FastEthernet 0/0 de salida, o en otras palabras, para que antes de salir por la interfaz se haga el bloqueo necesario. Se hizo uso de un ACL extendido que nos permitiera colocar dos especificaciones en una sola lista.

>```
>R1_Bog(config)#access-list (Numero identifiacdor de la ACL) deny tcp (IP de la Vlan) (Subnet Mask o Wild Card) any eq (Puerto)
>
>R1_Bog(config)#access-list (Numero identifiacdor de la ACL) permit ip any any
>
>R1_Bog(config)#interface (Interfaz que se vaya a trabajar)
>
>R1_Bog(config-if)#ip access-group (Numero identifiacdor de la ACL) (out/in)

Aplicandolo para el caso del laboratiorio: 

>```
>R1_Bog(config)#access-list 101 deny tcp 172.17.0.0 0.0.3.255 any eq 80
>
>R1_Bog(config)#access-list 101 permit ip any any
>
>R1_Bog(config)#interface Fa0/0
>
>R1_Bog(config-if)#ip access-group 101 out

Lo anterior nos generará el siguiente resultado:

>```
>R1_Bog# show access-lists

![ACCESS-LIST 101](/img/ACCESS-LIST%20101.jpg)

Para el R2_ESP se uso la misma lógica para la especificación del uso de los puertos 443 y 80 en las Vlan’s 25 (Tesorería) y 100 (Vice), pues la 25 solo podía hacer uso de el servicio HTTPS, pero, a diferencia que, en el caso anterior, la Vlan 100 no podría hacer uso de ningún servicio del servidor Web, por ello se decidió bloquear cualquier tipo de interacción de cualquier dirección perteneciente a la Vlan antes mencionada con el servidor. En este caso la interfaz que se usará, también como salida, será la Serial 0/1/1. Cabe recalcar que la dirección del servidor Web es 161.125.6.2

>```
>R2_Esp(config)#access-list 102 deny tcp 110.111.0.0 0.0.1.255 any eq 80
>
>R2_Esp(config)#access-list 102 deny ip 110.111.2.0 0.0.1.255 host 161 125.6.2
>
>R2_Esp(config)#access-list 102 permit ip any any
>
>R2_Esp(config)#interface Serial0/1/1
>
>R2_Esp(config-if)#ip access-group 102 out

>```
>R2_Esp# show access-lists

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

>```
>R2_Esp(config)#router EIGRP (Número de Identificación del protocolo)

Esto nos dará entrada a la configuración del protocolo en la que deberemos especificar las direcciones de identificación de todas las redes a las que el router este directamente conectado de la siguiente manera: 

>```
>R2_Esp(config-router)#newtork (IP identificadora de la red)(Wild Card)

Para el caso del laboratorio, la configuración fue la siguiente: 

>```
>R2_Esp(config)#router EIGRP 1
>
>R2_Esp(config-router)#newtork 216.169.1.2 0.0.0.3
>
>R2_Esp(config-router)#network 10.111.0.0 0.0.255.255

Después de hacer esto en cada uno de los routers con sus respectivas redes vecinas, el protocolo se encargará de hacer que cada uno de los routers aprendan las redes conectadas a cada uno de sus vecinos, completando sus tablas de enrutamiento para cada uno de los destinos necesarios.

A continuación, se puede ver como quedaría la guía de enrutamiento de uno de los routers, específicamente el R2_ESP:

![SHOW_IP_ROUTE-R2_ESP](/img/SHOW%20IP%20ROUTE%20-%20R2_ESP.jpg)

Podemos ver como la gran mayoría empieza con la letra D, lo que según las instrucciones del comando “show ip route” se refiere a las direcciones de redes aprendidas por medio del protocolo EIGRP, o en otras palabras, las redes que los otros routers le proporcionaron al router en el que nos encontramos, acompañadas de la dirección de la interfaz del dispositivo que las proporciono y por cual interfaz se recibió la información en este caso podemos ver que solo aparece la dirección del Serial 0/1/1 del router ISP_ESP porque es la única conexión que tiene el router R2_ESP con la red de routers. Además, podemos ver las direcciones de las redes que están directamente conectadas a el router.

# Verifiación
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

