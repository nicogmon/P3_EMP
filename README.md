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
En mi caso he creado 2 threads. 
En primer lugar he creado un thread que mide la distancia a tarves del ultrasonido 

