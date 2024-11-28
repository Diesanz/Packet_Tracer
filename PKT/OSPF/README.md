# OSPF

## Presentación

El Open Shortest Path First (OSPF) es un protocolo de enrutamiento dinámico jerárquico, diseñado para optimizar el intercambio de rutas dentro de redes complejas. Este protocolo organiza las redes en áreas, siendo el área 0 el backbone de OSPF, al cual se conectan todas las demás áreas. Los routers en OSPF desempeñan roles específicos dependiendo de su función:

* **ABR (Area Border Router):** Conectan el área 0 con otras áreas, manteniendo una base de datos independiente para cada área.
* **ASBR (Autonomous System Boundary Router):** Importan rutas desde otros dominios no OSPF al dominio OSPF.
![Ejemplo Ospf](/PKT/OSPF/imagenes/image.png)

## Consideraciones para Diseñar una Red Modular con OSPF

Antes de configurar OSPF, es esencial diseñar la topología de la red, identificando:

* Las áreas OSPF.
* La ubicación de los ABRs.
* Los ASBRs, si existen.

En un diseño modular jerárquico:

* El Core de la red generalmente corresponde al área 0.
* Los módulos (por ejemplo, distribución, acceso, WAN, o VPNs) se asignan a áreas específicas para segmentar y optimizar la red.
* Los ABRs suelen ubicarse en los routers de distribución para evitar sobrecargar los routers del Core.


Una topología típica podría estructurarse de la siguiente forma:

* **Core (Área 0):** Interconecta todos los módulos.
* **Módulos específicos:** Cada módulo puede pertenecer a un área diferente, dependiendo de las necesidades y el diseño de la red.

    ![alt text](/PKT/OSPF/imagenes/image2.png)

## Configuración del Módulo WAN y VPNs

Para los módulos que conectan redes internas (como WANs o VPNs), es crucial garantizar que todas las áreas tengan acceso al área 0. Esto plantea algunos desafíos de diseño:

**Extender el área 0 hasta los módulos remotos:**

* Permite interconectar módulos remotos directamente con el backbone OSPF.
* Puede generar un impacto negativo en el tiempo de convergencia y estabilidad del backbone si uno de los módulos remotos es inestable.

**Conectar módulos remotos al área 0 mediante un router concentrador:**

* El router concentrador sería un ABR, conectado tanto al área 0 como a las áreas de los módulos remotos.
* Es fundamental que el router ABR tenga suficiente capacidad para manejar la carga de varias áreas.

En este ejemplo, asumiremos una solución simplificada:

* El router concentrador de WAN o VPN será el ABR.
* Cada módulo remoto será parte de un área distinta, asegurando segmentación y escalabilidad.


## Resumen

El diseño modular jerárquico en OSPF permite escalabilidad, estabilidad y organización clara del dominio de enrutamiento. Este enfoque, combinado con una correcta ubicación de ABRs y ASBRs, optimiza la convergencia y la eficiencia de la red, proporcionando flexibilidad para adaptarse a los requisitos específicos de cada organización.

### [Realización Práctica](EjemploOspf.md)