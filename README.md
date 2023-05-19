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
  Y tambien se plantea La tabla de direccionamiento:
  ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/7ebcf7a3-ba03-46cc-a307-d708178b5179)


* Luego del subneteo correspondiente, se configura el servidor DHCP de esta manera:
* Posteriormente, se configuran los switch de la siguiente manera:
  1) Se asigna el nombre, las claves de la consola, vty y el modo privilegiado y el mensaje del día. (Todo Siempre se guardaba los cambios realizados con el comando "copy running-config startup-config"
  2) Se crean las Vlans con sus nombres correspondientes con "vlan #" y despues name (nombre de la vlan)
  3) Se asigna la Ip a la interface de mantenimiento, siendo la vlan nativa 99 con el comando "interface vlan 99" y "ip address (ip y su mascara de red)"
  4) Segun la tabla de asignacion de Vlanes que es requerida y solicitada por el usuario:
    ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/64dea675-bf35-4e22-b36b-720d225e6db5)
    Se activaron los puertos con su modo correspondiente con los comandos "interface range Fa0/#-0/# o Gig0/1-2" y siguiendo con el comando "switchport mode access", para los puertos de la vlan nativa, se seleccionaba sus puertos y se utilizaban los comando   "switchport mode trunk"
    Se Asignaron los puertos a cada vlan en cada switch con los comandos "interface range Fa0/#-0/# o Gig0/1-2", siguiendo con el comand "switchport access vlan ##", y para los puertos de la vlan nativa, se utilizo el comando "switchport trunk native vlan 99".
    Cabe aclarar que esta configuracion no aplica para los dispositivos con la configuracion de red inalambrica.

* La configuracion del router:
  1) Se asigna el nombre, las claves de la consola, vty y el modo privilegiado y el mensaje del día. (Todo Siempre se guardaba los cambios realizados con el comando "copy running-config startup-config"
  2) Se activa el puerto del router que se utilizara con el comando "interface Fa0/0"
  3) Se establece la encapsulacion de enlace y se asocia las Vlans con cada subinterfaz del router con los siguientes comandos "interface fastethernet 0/0.##(numero de la Vlan)" y encapsulation dot1q ##(numero de la vlan)
  
* Configuracion del access point a traves del controlador inalambrico de LANs
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
 ![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/baa0c037-a218-4e88-a244-e1a575c6702f)
14) Por ultimo, se conecto cada dispositivo a sus respectivas WLANs, para los laptop, habia que ingresar en el aplicativo de "PC wireless", seleccionar la WLAN e ingresar la contraseña para esa WLAn
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/d514fe57-f005-49a4-8ce3-8635a6a38203)
Para los otros dispositivos, hay que configurar manualmente la WLAN, dijitando el nombre y clave asi:
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/3f289d5e-4bd4-424f-9cec-bc4278e54c9a)
#### Comunicacion Socket TCP
Se codifico desde una plantila de codigo en el servidor web como el Servidor de Socket TCP y en cada computador como clientes del servidor Socket TCP:
![image](https://github.com/julian2308/ProyectoFinalRedes/assets/88839459/b6850b83-ad9c-435d-953a-cbb6b95676d0)

### Resultados y analisis
#### 1)	Evaluar el flujo bidireccional de datos generado al acceder a la página alojada en el servidor Web por los nodos terminales de las diferentes redes que conforman la topología de la Figura 1, utilizando el servicio DNS. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer.


#### 2)	Evaluar el flujo bidireccional de datos generado al acceder desde los nodos terminales, tipo smartphone y Laptop pertenecientes a la LAN Mi Casa “Inteligente”, a la interfaz gráfica de gestión de los dispositivos de Internet de las Cosas que se incluyen en Mi Casa “Inteligente”. Justifique su análisis utilizando capturas con el simulador y los filtros de paquetes de Cisco Packet Tracer.

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

* Red LAN: "Se conoce como red LAN (siglas del inglés: Local Área Network, que traduce Red de Área Local) a una red informática cuyo alcance se limita a un espacio físico reducido, como una casa, un departamento o a lo sumo un edificio" [1].
* VLAN: "Las VLAN o también conocidas como «Virtual LAN» nos permite crear redes lógicamente independientes dentro de la misma red física, haciendo uso de switches gestionables que soportan VLANs para segmentar adecuadamente la red" [2]. Básicamente permiten segmentar una misma red apartir de un Switch, y tener distintas subredes.
* IPv4: "(Internet Protocol versión 4) es el formato de dirección estándar que permite que todas las máquinas en Internet se comuniquen entre sí. IPv4 se escribe como una cadena de dígitos de 32 bits y una dirección IPv4 se compone de cuatro números entre 0 y 255, separados por puntos" [3].
* Spanning Tree: "Spanning Tree Protocol (STP) es un protocolo de capa dos publicado en la especificación IEEE 802.1.
El objetivo de STP es mantener una red libre de bucles. Un camino libre de bucles se consigue cuando un dispositivo es capaz de reconocer un bucle en la topología y bloquear uno o más puertos redundantes" [4].
* Subneteo: "Hace referencia a la subdivisión de una red en varias subredes. El subneteo permite a los administradores de red, por ejemplo, dividir una red empresarial en varias subredes sin hacerlo público en Internet. Esto se traduce en que el router que establece la conexión entre la red e Internet se especifica como dirección única, aunque puede que haya varios hosts ocultos. Así, el número de hosts que están a disposición del administrador aumenta considerablemente" [5].

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
* Curso de Skills For All Networking Essentials. Módulo 17 y 18. "https://skillsforall.com/launch?id=104c1536-83d6-47db-b7a1-6007327b3134"
