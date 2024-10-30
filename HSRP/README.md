#Practica 1 Ejercicio de Redes con VLAN y PVST

![Topología de Red](/imagenes_ejercicios/practica1.png)

En la figura anterior, en cada PC se indica en el nombre la VLAN a la que pertenece (la letra tras el ID de VLAN es sólo para que los nombres de los PCs sean distintos).

Una vez comprendidos ambos protocolos y tras haber replicado el ejemplo, desarrolle con él un caso de diseño de red en el que se utilice PVST en la siguiente situación:

En una LAN en la que existen varias VLANs, se quiere asegurar que, en ausencia de enlaces o conmutadores caídos, cada una de ellas recibe un cierto ancho de banda en los enlaces disponibles, priorizando a una(s) VLAN(s) frente a otras.

Además, la red debe ser funcional, permitiendo intercambio de paquetes entre las distintas VLANs y ofreciendo servicios de DHCP y DNS. Éstos deben ser soportados por un único servidor, lo que en el caso de DHCP supone configurar un proxy DHCP (o DHCP relay) en el/los router/s incorporados.