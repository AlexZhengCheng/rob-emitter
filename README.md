###### Grupo y nombre:
Exceptional Emergency Juggernauts
Bofeng Alejandro Zheng Cheng

# Actuador - Emitter

**1. DESCRIPCIÓN**

El **actuador Emitter** es un módulo de lanzamiento de comunicación que se utiliza para transmitir emisores de radio, puertos serie o señales de comunicación infrarroja. Para simular una comunicación bidireccional entre dos robots, un robot debe tener un *Emitter* y el otro un *Receiver*. Por lo tanto, en la simulación, estos dos robots se intercambian mensajes mutuamente. Pero tanto el robot emisor como el receptor, no pueden transmitirse mensajes a sí mismo, es decir, cuando el emisor manda un mensaje al *Receiver*, él no puede recibir datos.

![bg right:100%](https://github.com/AlexZhengCheng/rob-emitter/blob/main/actuadorEmitter.jpg)

Para la simulación de este actuador, se necesitará el sensor de *Receiver* ya que éste recibirá los mensajes transmitidos por el emisor. Es decir, el actuador *Emitter* estará rodeado por una esfera donde se emitirán los mensajes, cuando el receptor entre en esa esfera, captará el mensaje transmitido por el emisor, lanzando por pantalla: “*Hello!*” ( *Figura 1* ). Si en algún momento el receptor sale de esa esfera, se emitirá un mensaje en la pantalla: “*Communication broken!*” ( *Figura 2* ).

![](https://github.com/AlexZhengCheng/rob-emitter/blob/main/IN_EMITTER_hello.jpg)
> Figura 1.

![](https://github.com/AlexZhengCheng/rob-emitter/blob/main/OUT_EMITTER_Communicationbroken.jpg)
> Figura 2.


---

**2. VERSIÓN SIMULADA Y VERSIÓN REAL**

A día de hoy, los robots son como los seres humanos, que tienen un “cerebro” encargado de controlar su entorno. Mientras que, en los robots, su cerebro serán los sensores que recogerán la información y la mandarán al controlador, con finalidad de emitir una respuesta, llevada a cabo por los actuadores. Para ello, el actuador utilizado en esta simulación es *Emitter*, la cual, tanto en la versión simulada como en la versión real tienen variedad de simulación.

En la versión simulada, utilizando el programa Webots obteniendo resultados en un entorno controlado, y que consiste en la creación de un robot con su actuador *Emitter*, el cual, crea una esfera alrededor de él, emitiendo sus mensajes de comunicación. Cuando el sensor Receiver entre en algún momento a la esfera del emisor, recogerá el mensaje y lanzará por pantalla el mensaje recibido, y cuando salga de esa esfera, la comunicación entre ambos se romperá. Esta es la versión simulada por el programa mientras que en la versión real pueden influir en muchas variables, una de ellas puede ser utilizando sensores ópticos ( *Imagen 3* ) los cuales se encargan de detectar la cantidad de luz reflejada en la superficie. Está compuesto por un LED emisor infrarrojo generando la señal sobre la superficie, mientras que el fototransistor (detección de línea o proximidad) es el encargado de recibir la luz reflejada. 

Otro ejemplo, el sensor ultrasónico ( *Figura 4* ), se encuentra en el interior de los sensores para medir distancias o detectar obstáculos. Para su funcionamiento, dispone de dos pequeños cilindros, una trabaja como emisor, emitiendo ondas ultrasónicas, y la otra como receptor, que espera a que dicho sonido se emita y pueda recibirlo.

![](https://github.com/AlexZhengCheng/rob-emitter/blob/main/sensor%C3%93ptico.jpg)
> Figura 3.

![](https://github.com/AlexZhengCheng/rob-emitter/blob/main/sensorUltras%C3%B3nico.jpg)
> Figura 4.

---

**3. FORMA DE USO Y CONFIGURACIONES**

Para la creación de esta simulación usamos el actuador *Emitter*, además de utilizar otro sensor Receiver para el correcto funcionamiento de esta simulación. El actuador *Emitter* estará compuesto por una esfera donde transmitirá los datos al receptor y este recogerá el mensaje. Para lograr esta simulación, se deberá seguir los siguientes pasos:

* **Paso 1**: Crear un nodo de robot, dentro de este nodo añadimos el nodo Emitter en el apartado children.

* **Paso 2**: Creamos la forma de nuestra figura en el nodo secundario, en la clase children del nodo Emitter, donde podremos crear la forma tridimensional para el trasmisor.

* **Paso 3**: La configuración de los parámetros del transmisor:
	-	*Type* : se usa para restablecer los tipos de señales, que son: “Radio”, la comunicación inalámbrica más común, “Infra-Red”, la comunicación infrarroja; y “Serial”, la comunicación del puerto serie. Las comunicaciones infrarrojas serán bloqueadas por los obstáculos que hay en la simulación.
	
	-	*Range* : es el radio de la esfera que rodea al emisor, donde se emite la comunicación en metros. Si el Receiver se encuentra en esa esfera, éste puede recibir un mensaje. Y cuando el artículo tiene el valor.
	
	-	*maxRange* : valor máximo permitido para range, la cual define el valor máximo que se puede establecer mediante la función:
	`wb_emitter_set_range`
	
	-	*-	Channel* : el número de canal de comunicación del *Receiver* y el *Emitter* deben ser consistentes para establecer la comunicación. Para que el receptor reciba los mensajes transmitidos por el emisor, el receptor deberá estar en el mismo canal, además, se deberá de utilizar números positivos para que haya comunicación entre estos dos robots.
	
	-	*BaudRate* : es la tasa de baudios, que afectan a la velocidad de transferencia de datos. Cuando el valor es de -1, la velocidad de transmisión es infinita, por lo tanto, el paquete se alcanza al instante.
	
	-	*Aperture* : se mide en radianes y es el ángulo que define la apertura del cono de emisión para los infrarrojos, donde solo podrá enviar datos a los receptores cuando se encuentre dentro del cono del emisor. Para el funcionamiento de este campo, el vértice del cono en encontrará en el eje [0 0 0] del emisor y el eje del cono deberá coincidir con el eje x del emisor.
	
	-	*allowedChannels* : el emisor podrá emitir sólo a los canales permitidos.
	
	-	*byteSize* : para poder transmitir información dependerá del tamaño de byte, que es el número de bits necesarios para poder retransmitir.
	
	-	*bufferSize* : calcula el tamaño del búfer de transmisión en bytes, cuyo número de bytes en los paquetes en cola no podrá sobrepasar ese número. 

* **Paso 4**: Configurar las funciones del emisor:

	-	Para enviar un paquete de datos al receptor, incluiremos el paquete:
	
			 #include <webots/emitter.h> 
			 int wb_emitter_send(WbDeviceTag tag, const void *data, int size);
	
	La función *wb_emitter_send* mete a la cola del emisor un paquete de *size* bytes situada en la dirección indicada por **data.* Este paquete de datos enviará la información a los receptores a una velocidad indicada por el *baudRate* del emisor y el nodo receptor recibirá la información inmediatamente. La ejecución de esta función devolverá 1 si el mensaje está en la cola de envío, en caso contrario, si devuelve 0, quiere decir que la cola está llena. La consideración de que la cola esté llena es cuando la suma de bytes de todos los paquetes que se encuentra en cola sobrepasa el tamaño del campo búfer (*bufferSize*).
	
	-	Para establecer y obtener el canal del emisor, definiremos el paquete y las siguientes funciones:
	
			 #include <webots/emitter.h>
			 #define WB_CHANNEL_BROADCAST -1
			 void wb_emitter_set_channel(WbDeviceTag tag, int channel);
			 int wb_emitter_get_channel(WbDeviceTag tag);
	
	La función `wb_emitter_set_channel` permitirá el cambio de canal de transmisión, donde el campo del destinatario deberá estar incluido. Para ello, el canal *allowedChannels* tendrá que estar vacío, por lo tanto, se modificará el campo *channel* del nodo emisor.
El emisor envía datos a los receptores si se usa el mismo canal, pero si se utiliza el valor de `WB_CHANNEL_BROADCAST`, se podrá transmitir a todos los canales. Y la función wb_emitter_get_channel devolverá el número de canal donde se encuentre actualmente.

	-	Para establecer y obtener el rango del emisor, incluiremos las siguientes funciones:
	
			 void wb_emitter_set_range(WbDeviceTag tag, double range);
			 double wb_emitter_get_range(WbDeviceTag tag);
			 
	La función `wb_emitter_set_range` hace que el controlador pueda cambiar el rango de transmisión cuando se esté ejecutando, y además, modifica el campo del nodo *Emitter*. Si en algún momento el rango del argumento es más grande, entonces se establecerá como *maxRange*. Y la función `wb_emitter_get_range` devuelverá el rango del emisor.
	
	-	Para establecer y obtener el rango del emisor, incluiremos las siguientes funciones:
	
			 int wb_emitter_get_buffer_size(WbDeviceTag tag);
	Donde devolverá el tamaño del búfer de transmisión en bytes, correspondiente al valor especificado por el *bufferSize* del nodo *Emitter*. Este tamaño indicará el número de bytes de datos almacenados en la cola del emisor. Si el búfer está lleno, la función `wb_emitter_send`  no responderá y por lo tanto, devolverá 0.
