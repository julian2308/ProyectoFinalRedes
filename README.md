# Proyecto Final Redes

## Presentado por: Julián Alvarado, Julián Durán & Julián Pinilla.

### CONTEXTO DEL PROBLEMA.
Un cliente de tipo empresarial le solicita un servicio de instalación de redes a la empresa Julianes SAS, este cliente tiene como necesidad montar una red educativa para la universidad de la sabana, esta red, además de necesitar diferentes subredes de acuerdo a la persona que la esté usando, divididas de la siguiente manera, 3 departamentos de uso regular, siendo Estudiantes, Profesores y Una subred particular en la biblioteca, además de una subred de servicio técnico y nativa, solicitando 5 en total.
Administrando el tráfico de la red dentro de las instalaciones educativas y un teniendo un servidor DHCP para la optimización de conexión, tiene un punto de acceso inalámbrico, que permite la conexión de usuarios sin necesidad de los equipos propios de la universidad (Ubicados en la biblioteca). Por otro lado, este cliente también solicita una instalación de una red para la implementación del internet de las cosas en su casa (Una casa inteligente). Por último, se solicita que estas dos redes estén en comunicación entre sí y con unos servidores WEB y DNS. El cliente solicita que el montaje sea con la topología evaluada de acuerdo con su capital, siendo la siguiente:
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/7d5d8f1a-25bb-42bf-a399-8ff488606153)


### MONTAJE
#### Red Campus
* Red “Campus”: 128.10+X.0.0/16: Con la topologia solicitada, primero verificamos la capacidad de la Red :
![Imagen de WhatsApp 2023-05-18 a las 11 43 59](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/9bab2a28-0e32-470a-9688-77cae3bb5d3c)
  Después, Se formula un sistema de direccionamiento de acuerdo a las Vlans solicitadas, con 950 posibles host, las VLan 20,35 y 40, y con 254 posibles host, las Vlans 55 y 99, buscamos la mascara de red mas optima usando el metodo de bits aprendido en clase:
  ![Imagen de WhatsApp 2023-05-18 a las 11 44 00](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/232a3f09-dd56-46cb-afc6-dce8cb9f8a1a)
  Se plantea la division de subredes con sus respectivos intervalos de Host:
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/afbc89d3-9d89-4511-84fe-c2ac132f2ecf)
  Y tambien se plantea La tabla de direccionamiento para toda la topología:
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/7ebcf7a3-ba03-46cc-a307-d708178b5179)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/71e62921-2fea-48e0-ab0d-30d450b969fe)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/22a9b7ee-a9e6-4b42-9dde-e30af719919f)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/20e68c62-123d-4670-a87f-011972a8aa2e)


* Posteriormente, se configuran los switch de la siguiente manera:
  1) Se asigna el nombre, las claves de la consola, vty y el modo privilegiado y el mensaje del día. (Todo Siempre se guardaba los cambios realizados con el comando "copy running-config startup-config"
  2) Se crean las Vlans con sus nombres correspondientes con "vlan #" y despues name (nombre de la vlan)
  3) Se asigna la Ip a la interface de mantenimiento, siendo la vlan nativa 99 con el comando "interface vlan 99" y "ip address (ip y su mascara de red)"
  4) Segun la tabla de asignacion de Vlanes que es requerida y solicitada por el usuario:
    ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/64dea675-bf35-4e22-b36b-720d225e6db5)
    Se activaron los puertos con su modo correspondiente con los comandos "interface range Fa0/#-0/# o Gig0/1-2" y siguiendo con el comando "switchport mode access", para los puertos de la vlan nativa, se seleccionaba sus puertos y se utilizaban los comando   "switchport mode trunk"
    Se Asignaron los puertos a cada vlan en cada switch con los comandos "interface range Fa0/#-0/# o Gig0/1-2", siguiendo con el comand "switchport access vlan ##", y para los puertos de la vlan nativa, se utilizo el comando "switchport trunk native vlan 99".
    Por ultimo con el codigo "spanning-tree mode rapid-pvst" Para activar el protocolo PVRST (Per VLAN Rapid Spanning Tree).
    
    ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/d81cdbc4-6540-43fe-a0e2-c63a3f6eafe9)

    Cabe aclarar que esta configuracion no aplica para los dispositivos con la configuracion de red inalambrica.

* La configuracion del router campus (R1 campus):
  1) Se asigna el nombre, las claves de la consola, vty y el modo privilegiado y el mensaje del día. (Todo Siempre se guardaba los cambios realizados con el comando "copy running-config startup-config".
  2) Se activa el puerto del router que se utilizara con el comando "interface Fa0/0".
  3) Se establece la encapsulacion de enlace y se asocia las Vlans con cada subinterfaz del router con los siguientes comandos "interface fastethernet 0/0.##(numero de la Vlan)" y encapsulation dot1q ##(numero de la vlan).
  4) Se activa el puerto del router que se utilizara con el comando "interface Serial0/2/0".
  5) Se le asigna una dirección ip para poder comunicarse con el router ISP que en este caso para esa interfaz seria 11.130.4.1.
  6) escribimos el comando "#router rip version 2" en nuestra terminal para habilitar el protocolo ripv2 y habilitar la comunicación entre routers.
  7) Usamos el comando "#network *ip*", el cual nos permite decirle al router ISP las redes que poseemos. En este caso, el router R1-Campus posee la red 128.12.12.0/16 en la interfaz Fa0/0 con sus respectivas vlans y subredes. Tambien, como se menciona anteriormente, en su interfaz Serial0/2/0 esta directamente conectada la red 11.130.4.1.
  8) Guardamos la configuracion con el comando "#copy running-config startup config" y procedemos a verificar que el routing para este dispositivo se haya realizado correctamente.

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/afeca67b-bff2-4494-9361-2c228ab16936)

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/0682bca2-8fed-430e-af7f-68b4f72452d4)

* Configuración del ruter isp (ISP):
  1) Se asigna el nombre, las claves de la consola, vty y el modo privilegiado y el mensaje del día. (Todo Siempre se guardaba los cambios realizados con el comando "copy running-config startup-config".
  2) Se activa el puerto del router que se utilizara con el comando "interface Serial0/3/0" y le asignamos su dirección ip de 11.130.4.2 255.255.255.252 el cual permitira la conexión con el router "R1 Campus".
  3) Se activa el puerto del router que se utilizara con el comando "interface Fa0/0" y le asignamos su dirección ip de 209.175.52.1 255.255.255.0 que es el defaul Gateway para la red de servidores y permitira su acceso desde la red campus.
  4) Se activa el puerto del router que se utilizara con el comando "interface Fa0/1" y le asignamos su dirección ip de 11.130.4.5 255.255.255.252 el cual permitira la conexión del ISP router a Cloud y a la red "mi hogar".
  5) De la misma forma que en "R1 campus", activamos la version 2 del protocolo de routing con el comando "#router rip version 2" habilitando la conexion al router "R1 campus".
  6) nombramos todas las redes existentes a las que nuestro router ISP tiene conexión en sus terminales (network 11.130.4.2 para la conexion al router R1 campus, network 209.175.52.1 para conexiones con servidores   DNS y HTTP y network 11.130.4.5 para la conexión con Cloud que posteriormente conecta a red Mi hogar).
  7) Volvemos a realizar el comando "#show ip route" para comprobar que realizamos efectivamente el procedimiento en nuestras redes. dado que ya hicimos este proceso con R1 campus, también nos aparecerán las redes que no estan conectadas directamente a nuestro ISP router pero si la interfaz mediante la que debe enviar los paquetes para acceder a la red campus. 

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/05147b82-ee1e-4be1-addc-6bd1092bfd8b)

Como se aprecia en la imagen, enfrente de las redes conectadas nos aparecen diferentes letras. C son para redes directamente conectadas, L para redes locales, y R para redes remotas, indicandonos también la via para acceder a ella.

  
* Se configura el servidor Web y el servidor DNS.
  1) Teniendo el servidor Web, inicialmente se le asigna una Ip estática que se encuentra dentro del rango determinado, y el default gateway a la interface correspondiente del router ISP.
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/f2b4db4d-54dd-4097-a931-2da720eac83b)
  2) Posteriormente activamos el servicio HTTP, y modificamos el index.html para tener nuestra página web personalizada.
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/c3455805-942e-4f1e-83bb-e002a8ae355d)
  3) De la misma forma asignamos una Ip estática al servidor DNS. Esta se utilizará en toda la topología como DNS Server.
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/7456c20a-3817-4eea-b4a7-cbd5b861c82b)
  4) Se activa ahora el servicio DNS, y se le asigna la url con las iniciales a la dirección Ip del servidor web.
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/8ae2927c-6553-48ec-b3c2-2971a7a4b844)

* Luego del subneteo correspondiente y configuración de los router R1 e ISP, se configura el servidor DHCP de esta manera:

  1) Se conecta el servidor DHCP al router al puerto correspondiente a la Vlan solicitada (Vlan 55).
  2) Se le asigna una Ip estática perteneciente a la Vlan 55 y se le asigna el default gateway correspondiente
     ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/9e7fa420-f512-4d16-b10a-dc4c3ec10eb8)
     ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/7ed46575-c0e7-46ab-9e8b-40bd2073e633)
  3) Posteriormente, se inicializa el servicio DHCP, y se crean las pools correspondientes a cada Vlan, en las cuales se les específica el default gateway dependiendo cuál Vlan es, y el rango de direcciones IP,            finalmente a todas se les asigna el mismo DNS.
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/f59ef0ab-7fcb-453d-ad2f-e82640433690)
  4) Después, desde el CLI del Router de la red campus, teniendo permisos de administrador, se le notifica a cada Vlan cuál de los pool debe utilizar para asignar sus características através del protocol DHCP.
  5) Através del comando interface FastEthernet 0/1."numero de la Vlan" accedemos a la configuración de la Vlan.
  6) Finalmente con el comando ip helper-address 128.12.12.10 (dirección Ip del servidor DHCP) se le asigana a cada Vlan el pool correspondiente.
  
  * ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/8ded18f9-11be-4b9c-8135-c398c8fca0f2)

* Configuración red "Mi casa":
  1) Inicialmente, es pertinente haber realizado la conexión entre cloud e ISP para poder proveer internet a la red de mi casa. La conexión de ISP a Cloud es recibida en Cloud en el puerto "Ethernet6".
  2) Pasamos ahora al cable modem, el cual se encargara de recibir y retransmitir el servicio de internet al "homegateway". Desde el Cloud conectamos con un cable coaxial al puerto "coaxial7" el cual conectaremos directamente al cable modem.
  3) En el cloud una vez habiendo conectado ambos dispositos ( ISP router y cable modem), establecemos que todo el trafico que llegue desde la interfaz "coaxial7" sea redirigida al puerto "Ethernet6" y viceversa. De esta forma lograremos conectividad a los servidores desde la red "mi casa".
  4) Una vez hecho esto, establecemos desde cable modem hasta HomeGateway que será el dispositivo que configurara todos nuestors dispositivos IoT.
  5) Para la configuración del HomeGateway, declaramos una dirección IP, mascara de subred, default gateway ( 11.130.4.5, interfaz0/0 de ISP router) y servidor DNS ( 209.175.52.3, dirección donde se aloja el servicio de DNS) para la interfaz Internet, que es donde recibiremos el servicio de internet de nuestro ISP. 
  6) Para configurar ahora si nuestra red mi casa, en la interfaz "LAN", establecemos el default gateway de todos los dispositivos de nuestro hogar (192.168.25.1) como la dirección IP del dispositivo y su respectiva máscara de subred.
  7) Para cada dispositivo, basta con establecer el protocolo DHCP para que todos obtengan su IP y máscara de subred de forma automática y, asimismo, se registren estos dispositivos en el servidor de IoT devices asociado a su default gateway. Para los dispositivos como Laptop-Hogar, smartphone-Hogar y Pc-hogar basta acceder a su interfaz "Wireless0" en su configuración y agregar el SSID de la red para poder conectarse inalámbricamente a la red y poder acceder y administrar el servidor IoT de hogar.

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/2133c742-0209-4a07-8312-75317d203336)

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/30f26875-546b-4045-a372-93ee8bec6319)
  
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/dd7ef2b2-bdae-43f8-842e-e32130c7e31a)
  
  Como se aprecia en la imagen, accediendo al servidor IoT asignado a la red "mi hogar", se nos despliegan todos los dispositivos conectados a nuestra red y de la misma forma nos permiten interactuar encendiéndolos o apagándolos.
  
* Configuración del access point a traves del controlador inalambrico de LANs
 1) Se conecta el controlador como la topologia lo indica
 2) Se asigna una IP con su respectiva mascara al controlador
 3) A traves de un PC conectado a al controlador, se entra a el navegador y en el buscador se ingresa: http://#.#.#.#(IP asignada al controlador), para asi entrar a la configuracion inicial del controlador.
 4) Se crea usuario y contraseña y se ingresa a la configuracion
 5) Se registra el controlador, Con datos como su ip y mascara
 6) Ya registrado, con cualquier PC conectado a la red campus, se entra a el navegador y en el buscador se ingresa otra vez pero cambiando el protocolo http a https: https://#.#.#.#
 7) Se ingresa con usuario y contraseña ya creadas y se ingresa al aplicativo completo de configuracion del controlador
 8) En la seccion de interfaces del controlador, se crearon las interfaces inalambricas, con su propio servidor de DHCP, dandoles su respectiva ip, mascara de subred, puerta de 
 enlace y IP para su propio servidor DHCP
 
 ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/dd7714ae-0af1-46e3-8b9a-1dd5d647e63b)
 
 ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/56b139c5-3e4e-4c70-87e8-1eb1a7c93aba)
 
 9) Se Crearon las redes WLAN a partir de las interfaces creadas anteriormente, con seguridad WPA2 y clave PSK como Clave de autenticacion
 ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/84c75ac0-8db0-4a76-af29-e35a94482e97)
 10) Se creo el propio servidor DHCP para la VLAN nativa (99)
 ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/7f3d8079-0bd3-4e04-9caf-633584779094)
 11) En la consola del LWAP(Light weight access point) se asigno el controlador ya configurado como su controlador y por el protocolo DHCP, se le asigno su ip, mascara de subred, puerta de enlace y DNS.
 12) Se excluyen las direcciones IP que se quieren usar en las subredes WLAN mediante los comandos "ip dhcp excluded-address (direcciones IP que se quieren exluir para conexiones inalambricas) en el router.
 13) Para crear los servidores DHCP para las VLANS que necesitan tecnologia inalambricas, siendo las VLANS 20, 40 y 55. Se crearon en el router, con la siguiente secuencia de comandos: "ip dhcp pool WLAN-##(numero de VLAN)", "network (Id de la Vlan con su mascara de subred)", "default-router (puerta de enlace de la VLAN)" y "dns-server (servidor DNS).
* ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/baa0c037-a218-4e88-a244-e1a575c6702f)
14) Por ultimo, se conecto cada dispositivo a sus respectivas WLANs, para los laptop, habia que ingresar en el aplicativo de "PC wireless", seleccionar la WLAN e ingresar la contraseña para esa WLAn
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/d514fe57-f005-49a4-8ce3-8635a6a38203)
Para los otros dispositivos, hay que configurar manualmente la WLAN, dijitando el nombre y clave asi:
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/3f289d5e-4bd4-424f-9cec-bc4278e54c9a)
#### Comunicacion Socket TCP
Se codifico desde una plantila de codigo en el servidor web como el Servidor de Socket TCP y en cada computador como clientes del servidor Socket TCP:
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/b6850b83-ad9c-435d-953a-cbb6b95676d0)

### Resultados y analisis
#### 1)	Evaluar el flujo bidireccional de datos generado al acceder a la página alojada en el servidor Web por los nodos terminales de las diferentes redes que conforman la topología de la Figura 1, utilizando el servicio DNS. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer.

Ya teniendo toda la topología correctamente montada y configurada, se puede acceder a la página web con la url asignada gracias al servidor DNS.

* Desde la LAN de la casa.
* ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/3b841798-edf4-45a2-a203-93c638e1034a)

* Desde campus.
* ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/c13aa869-772c-47fb-86b3-37d782e07ae9)
* ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/c3a5a680-9b14-4489-baee-c5f0a0ae9f92)

La comunicación con la página web es através del protocolo (HTTP), el cual, como bien sabemos funciona con la arquitectura cliente - servidor, esta se caracteriza por poseer como bien lo dice su nombre, un cliente y un sevidor. El cliente realiza peticiones al servidor, y este, responde a estas peticiones una a una, tanto como si funcionó, devolviendo el archivo HTML correspondiente a la ip, como si no funcionó, con una respuesta de código de estado 404 NOT FOUND. Finalmente, para una mejor experiencia de usuario, se usa el servicio DNS, el cual se encarga de convertir las request enviaadas por el cliente en formato de URL, en direcciones Ip's que están asociadas a esta misma, y que los dispositivos son capaces de entender.
* ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/64561271/b8d65dcc-b671-4d50-bc29-c65bab7cab59)





#### 2)	Evaluar el flujo bidireccional de datos generado al acceder desde los nodos terminales, tipo smartphone y Laptop pertenecientes a la LAN Mi Casa “Inteligente”, a la interfaz gráfica de gestión de los dispositivos de Internet de las Cosas que se incluyen en Mi Casa “Inteligente”. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer.

Mediante cualquiera de los dipositivos inalámbricos (pc-Hogar, Smartphone-hogar o Laptop-Hogar) es posible acceder a la interfaz de dispositiovs IoT.
 - Para Pc-Hogar:
 - 
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/f5c69567-381d-45bf-a203-d0f5290b3a46)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/44089c88-28af-44d2-a46d-2f4a0d7e13e3)

- Para Smartphone-Hogar:
- 
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/af8647a6-f2ff-452c-b1b0-869204cafdcf)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/4473c520-0ac7-4421-9442-88d8b8c7522f)

- Para Laptop-hogar:
- 
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/a1ef3c31-77f1-47a7-b9eb-6120389260d3)
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/d739bdfc-2c1e-477a-98b2-abcdb48bb1c2)

Al intentar acceder desde alguno de los nodos terminales, independiente de cual sea, los paquetes siguen el msimo patrón. Desde el nodo terminal del cual se accede al servidor se manda un paquete al homegateway.

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/c385ca02-eda1-41ce-b743-08b4311a4aee)

A partir de ahí, dependiendo del dispositivo que se active/desactive, el Homegateway manda un mensaje broadcast a toda la red para que el determinado dispositivo sea accionado mediante el servidor.

  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/110574175/51ea0088-a7b7-4b7f-8732-17f08a894150)

#### 3)	Evaluar el flujo bidireccional de datos generado por los PCs de la red “Campus” al ejecutar el script cliente basado en socket TCP y al conectarse con el servidor socket-TCP corriendo en el Servidor Web. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer.
Como primera Instancia, al poner a correr el socket TCP desde un computador y el servidor, siendo el servidor el que corre primero, el computador manda un paquete de capa 4, desde el puerto del computador al puerto del servidor Web con estos propositos y especificaciones.

![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/0b22c278-6920-4518-8efe-375381447918)

Siendo un paquete de tipo TCP, ya que se define no solo una IP, si no un puerto destino en especifico 
Al llegar a el servidor WEB, podemos ver que recibe la informacion y inmediatamente cierra la conexion TCP

![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/2f8e3daf-c2a1-4775-90b1-bb9b630b321e)

Se puede intuir que es debido al numero de secuencia en el packete TCP, que cuando es 1, la comunicacion TCP se desconecta, mientras que, cuando es 0, se recibe y se acepta la conexion TCP asi:

![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/2330ee43-8c8c-46f3-b290-9b8182fef581)

Por ultimo, el PC recibe una respuesta que la conexion TCP del socket due establecida

![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/f4332d84-596e-4d8e-9b13-fb6f9094f5c8)

Se puede evaluar que esta comunicacion esta funcionando como lo esperado y sin fin, ademas de demostrar unos aspectos interesantes, por ejemplo, la conexion TCP entre el servidor y el cliente es de capa 7:

![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/b9549086-72fb-4604-b24b-e2da60553b31)

Pero toda la comprobacion de IP y puertos se realiza en la capa 4.






### METODOLOGÍA SEGUIDA.
Estos son los conceptos, protocolos y servicios aplicados en el desarrollo.
* VLAN: "Las VLAN o también conocidas como «Virtual LAN» nos permite crear redes lógicamente independientes dentro de la misma red física, haciendo uso de switches gestionables que soportan VLANs para segmentar adecuadamente la red" [2]. Básicamente permiten segmentar una misma red apartir de un Switch, y tener distintas subredes.
* IPv4: "(Internet Protocol versión 4) es el formato de dirección estándar que permite que todas las máquinas en Internet se comuniquen entre sí. IPv4 se escribe como una cadena de dígitos de 32 bits y una dirección IPv4 se compone de cuatro números entre 0 y 255, separados por puntos" [3].
* Spanning Tree: "Spanning Tree Protocol (STP) es un protocolo de capa dos publicado en la especificación IEEE 802.1.
El objetivo de STP es mantener una red libre de bucles. Un camino libre de bucles se consigue cuando un dispositivo es capaz de reconocer un bucle en la topología y bloquear uno o más puertos redundantes" [4].
* Subneteo: "Hace referencia a la subdivisión de una red en varias subredes. El subneteo permite a los administradores de red, por ejemplo, dividir una red empresarial en varias subredes sin hacerlo público en Internet. Esto se traduce en que el router que establece la conexión entre la red e Internet se especifica como dirección única, aunque puede que haya varios hosts ocultos. Así, el número de hosts que están a disposición del administrador aumenta considerablemente" [5].
* HTTP: "Es el nombre de un protocolo el cual nos permite realizar una petición de datos y recursos, como pueden ser documentos HTML" [6].
* DNS: "Corresponde a las siglas en inglés de "Domain Name System", es decir, "Sistema de nombres de dominio". Este sistema es básicamente la agenda telefónica de la Web que organiza e identifica dominios. estas siendo protocoles de la capa de aplicación en el proceso de encapsulamiento y des encapsulamiento de datos" [7].
* TCP: "TCP (Protocolo de Control de Transmisión, por sus siglas en inglés Transmission Control Protocol) es protocolo de red importante que permite que dos anfitriones (hosts) se conecten e intercambien flujos de datos, este siendo el protocolo de la capa de tranporte" [8].
* IP: "Protocolo de internet, que permite identificar un dispositivo en una red, siendo el Router el generador de esa IP, y ese router asigna a cada dispositivo conectado a la red su propia IP a través del protocolo" [9].
* DHCP: "(Protocolo de configuración dinámica de host) o también conocido como «Dynamic Host Configuration Protocol« , asignando las direcciones lógicas de cada dispositivo, al ser la capa de red en el modelo TCP/IP" [10].
* Red LAN: "Se conoce como red LAN (siglas del inglés: Local Área Network, que traduce Red de Área Local) a una red informática cuyo alcance se limita a un espacio físico reducido, como una casa, un departamento o a lo sumo un edificio" [11].
* Socket: "Los sockets son canales de comunicación que permiten que procesos no relacionados intercambien datos localmente y entre redes. Un único socket es un punto final de un canal de comunicación bidireccional" [12].
* WAN: "Una red de área amplia (WAN) es la tecnología que conecta entre sí a las oficinas, los centros de datos, las aplicaciones en la nube y el almacenamiento en la nube" [13].

Dispositivos utilizados:

- PC-PT.
- Access Point (3702i).
- Server-PT.
- Wireless Lan Controller1 (WLC-3504).
- Switch (2960).
- Laptop-PT.
- Router (2811).
- Cloud-PT
- Cable Modem.
- Smartphone
- Light
- Fan
- Webcam
- Appliance
- Door
- Window
- Garage Door

### AGENDA PROYECTO REDES.
#### Martes 9 de mayo.
Análisis de la topología a montar, lectura y entendimiento del documento, repartición básica de las tareas.
Alvarado: Servidor DNS y Web.
Durán: IOT LAN.
Pinilla: Montaje básico de la red campus.
#### Viernes 12 de mayo.
Reporte de avances, corrección de conexiones en red campus y pruebas de red IOT. Las tareas asignadas en la reunión anterior fueron completadas con éxito.
Alvarado: Configuración DHCP en red campus.
Durán: Conexión Serial entre campus e ISP.
Pinilla: Conexión entre Cloud e ISP.
#### Lunes 15 de mayo.
Reporte de avances, debugeo en conjunto del servicio DNS para la conexión desde PC pertenecientes a la red campus. Únicamente falla conexión entre Cloud e ISP, resto de tareas completadas correctamente.
Alvarado & Durán: Revisión conexión Cloud e ISP.
Pinilla: Conexión y configuración del Access Point.
#### Jueves 18 de mayo.
Corrección final de errores y grabación del vídeo.


### REFERENCIAS
* [1] Fragmento tomado de: "https://concepto.de/red-lan/#ixzz7xxuZ6gL7"
* [2] Fragmento tomado de: "https://www.redeszone.net/tutoriales/redes-cable/vlan-tipos-configuracion/"
* [3] Fragmento tomado de: "https://www.avg.com/es/signal/ipv4-vs-ipv6#:~:text=IPv4%20(Internet%20Protocol%20versi%C3%B3n%204,y%20255%2C%20separados%20por%20puntos."
* [4] Fragmento tomado de: "https://aprenderedes.com/2019/11/protocolo-de-arbol-de-extensionstp/"
* [5] Fragmento tomado de: "https://www.ionos.es/digitalguide/servidores/know-how/subnetting-como-funcionan-las-subredes/"
* [6] Fragmento tomado de: "https://developer.mozilla.org/es/docs/Web/HTTP/Overview"
* [7] Fragmento tomado de: "https://developer.mozilla.org/es/docs/Glossary/TCP"
* [8] Fragmento tomado de: "https://support.google.com/a/answer/48090?hl=es-419#:~:text=DNS%20corresponde%20a%20las%20siglas,que%20organiza%20e%20identifica%20dominios."
* [9] Fragmento tomado de: "https://www.avast.com/es-es/c-what-is-an-ip-address"
* [10] Fragmento tomado de: "https://www.redeszone.net/tutoriales/internet/que-es-protocolo-dhcp/"
* [11] https://developer.mozilla.org/es/docs/Learn/Server-side/First_steps/Client-Server_overview
* [12] Fragmento tomado de: https://developer.mozilla.org/es/docs/Learn/Server-side/First_steps/Client-Server_overview
* [13] Fragmento tomado de: "https://aws.amazon.com/es/what-is/wan/#:~:text=Una%20red%20de%20%C3%A1rea%20amplia%20(WAN)%20es%20la%20tecnolog%C3%ADa%20que,el%20almacenamiento%20en%20la%20nube."
* Curso de Skills For All Networking Essentials. Módulo 17 y 18. "https://skillsforall.com/launch?id=104c1536-83d6-47db-b7a1-6007327b3134"
