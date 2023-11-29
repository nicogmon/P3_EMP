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
