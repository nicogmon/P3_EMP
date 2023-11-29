# P3_EMP
Para esta practica afrontamos la tarea de simular el centro de control de una maquina dispensadora de cafe.  
Para ello utilizaremos los siguientes componenetes hardware:  
&emsp;● Arduino UNO  
&emsp;● LCD  
&emsp;● Joystick  
&emsp;● Sensor temperatura/Humedad DHT11  
&emsp;● Sensor Ultrasonido  
&emsp;● Boton  
&emsp;● 2 LEDS Normales (LED1, LED2)    
Sigueindo el siguiente esquema:   
(El senso de temperatura DHT11 carece del cuarto pin mostrado en el esquema y cambia el orden de los pines respescto al esquema generado)  
![Screenshot from 2023-11-29 19-19-03](https://github.com/nicogmon/P3_EMP/assets/102520722/79e11738-7995-4eb1-87a5-8486c339624a)

Detalles Hardware:  

Los pines asociados al boton simple y al boton del joystick se han elegido correspondiendo a los pines que disponen de una resistencia pullup interna en nuestra placa, pines 2 Y 3.  
Por otra parte, el led1 ha sido conectado a un pin analogico debida la falta de pines digitales suficientes para todos los componentes, no limitanod esto su comportamiento ya que solo debia encenderse y apagarse a una luminosdiad fija.  
Por ultimo el led1 ha de ser conectado a un pin con funcionalidad pwm para poder regular su intensidad como se especifica en el enunciado.


Implementacion software:  

Para la implementacion como estructura general he usado una estructura a base de switchs, implementando una maquina de estados.  
De esta manera he podido diferenciar entre estado de Servicio y estado Administrador, para los cambios de estado usamos diferentes medidas de los sensores, como puede ser la pulsacion del boton, o la finalizacion de tareas como la preparacion del cafe.

Para el desarollo mas concreto de la practica he utilizo diferentes herramientas software:

Interrupciones por hardware:  

&emsp;En el boton simple utilizo una interrupcion que detecte cada vez que el boton cambia de estado para asi poder medir dependiendo de si se pulsa o se sulta el tiempo que ha pasado el boton pulsado(se puede observar el codigo concreto en la funcion button). Ademas añado medidas para evitar el rebote que sufre este sensor y que hacia imposible el uso correcto de esta funcionalidad, para ello descartamos señales que tengan menos de 700 milisegundos de diferencia.
&emsp;Este mismo metodo anti rebote lo uso para el boton del joystick, aunque en este caso como mi interes se centraba en saber si se habia pulsado o no el boton, la interrupcon de este hacia de interruptor con una variable que cambiaba de 0 a 1 y viceversa dependiendo del estado que estuviera. De esta manera consigo que la pulsacion me de informacion prolongada en el timeppo y no durante lo que dura la pulsacion.  

Arduino threads:  

En mi caso he creado 3 threads. 
En primer lugar he creado un thread que mide la distancia a tarves del sensor de ultrasonidos para asi poder regular yo cada cuento tiempo queria realizar dicha medcion sin generar un delay que pudiera afectar a la fluidez mostrada por panatalla de los valores ni dejar el programa bloqueado.  

De la misma manera con el sensor de temperatura utilizo un thread ya que no me interesa medir la temperatura en intervalos tan pequeños de timepo y para utilizo un thread que se ejcuta cada 10 segundos de forma que reaccione adecuadamente ante un cambio pero no este constantmente leyendo dichos datos los cuales no varian apenas.  

Por ultimo he creado un thread que lleva el contador usando la funcion millis y que se ejecuta cada segundo porque es realmente la medida de segundos la que necesitamos y no es necesario leer dicho valor cada 100-200 milis generando un mayor consumo y uso del core.  

Watchdog:  
He utilizado el watchdog con temporizador de 8 segundos para evitar posibles bloqueos en whiles o en las diferentes acciones que se ejcutan durante e programa he tenido que establecer los resets al final del bucle principal y dentor de algunos subbloques con esperas o bucles que pueden tardar mas de este timepo establecido pero que no deberian generar bloqueos.

Como extra intente implementar que si durante el servicio la persona dejar de estar frente a la maquina se volviera a la deteccion de persona pero esto generaba comportamientos indeseados por malas detecciones o ruidos en la informacion recogida por le sensor.  
Aqui el codigo del intento de implementacion:  
```c++
if (meds_count == 10){
          
          qsort(meds, 10, sizeof(long), cmp_asc);
          mean = meds[5];
          Serial.print("mean ");
          Serial.println(mean);
          for (int i = 0; i < 10; i++){
            
            meds[i] = 0;
          }
}
```
con este codigo calculabamos la mediana de las ultimas 10 medidas y si esta era mayor a 100 volviamos a la detccion de persona.
No he conseguido que termine de funcionar de forma completamente satisfactoria aun produciendo errores asi que se encuentra comentado en el codigo.


A continuacion dejo videos de su funcionamiento:  
Funcionamiento de las funcionalidades de arranque y servicio:  


https://github.com/nicogmon/P3_EMP/assets/102520722/093339bc-7caa-416b-b951-28110de335cf  

observamos como se ejcuta el arranque, acto seguido pasa a esperar a que aparezca alguien a menos de un metro y observamos como mediante el joystick se eligen adecuadamente las bebidas y se ejecuta su "preparacion" con los leds indicadores y el mensjae en la pantalla tras el segundo cafe se observa bien que tras servir el cafe vuelve a comprobar si hay cliente si el cliente sigue estando la deteccion es muy rapido y por tanto apenas se alcanza a ver el mensaje de espera.

Funcionamiento del reinicio y el modo admin:  


https://github.com/nicogmon/P3_EMP/assets/102520722/5bc6664d-9a78-4b3e-905c-c9c7dc29d23c

En este video vemos como al iniciar reiniciamos la funcionalidad de servicio pulsando el boton durante 2 segundos y despues inciamos y probamos las diferentes opciones de admin y acabamos saliendo de dicho menu viendose como volvemos a la funcionalidad de servicio.  
Tras modificar el precio se mantiene mientras la placa siga encencida:  




