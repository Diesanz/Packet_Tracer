# Multilayer switching

## Presentación
Los switches multilayer son dispositivos de red que combinan las funciones de un switch de capa 2 y un router de capa 3, lo que les permite realizar tanto switching (enlace de datos) como routing (red) en un solo dispositivo. Esto ofrece varias ventajas en la arquitectura de redes modernas:

## Funcionalidad de Capa 2 y Capa 3

Los switches multilayer pueden realizar operaciones de conmutación de paquetes (switching) a nivel de capa 2 (como VLANs) y enrutamiento de paquetes a nivel de capa 3 (IP routing). Esto significa que pueden transferir tráfico dentro de una misma red local y también entre diferentes redes.

## Rendimiento Mejorado

Al integrar las funciones de switching y routing, los switches multilayer pueden procesar el tráfico de manera más eficiente. Esto reduce la latencia y mejora el rendimiento general de la red, ya que minimiza el número de dispositivos necesarios para realizar estas tareas.

## Segmentación de Redes

Permiten la creación de múltiples VLANs, lo que mejora la seguridad y el control del tráfico dentro de la red. Cada VLAN puede ser enrutada de manera independiente, lo que facilita la gestión del tráfico y ayuda a contener problemas dentro de una VLAN específica.

## Facilidad de Configuración

La configuración de VLANs y el enrutamiento en un switch multilayer suelen ser más simples en comparación con la necesidad de un router independiente y switches de capa 2. Los administradores de red pueden gestionar la red desde un solo dispositivo, lo que ahorra tiempo y recursos.

## Soporte para Protocolos Avanzados

Los switches multilayer suelen ser compatibles con protocolos de red avanzados como el Protocolo de Spanning Tree (STP), HSRP (Hot Standby Router Protocol) y otros protocolos de enrutamiento dinámico como RIP y OSPF. Esto permite una gestión de red más robusta y resiliente.

## Implementación en Redes Empresariales

Son ideales para redes empresariales grandes y complejas donde se requiere tanto la segmentación de tráfico como el enrutamiento entre diferentes subredes. Esto incluye entornos donde se requiere alta disponibilidad y balanceo de carga.

## Interfaz Virtual de Switching (SVI)

En un switch multilayer, la configuración de la dirección IP de las VLANs se realiza a través de interfaces virtuales llamadas SVIs (Switched Virtual Interfaces), lo que simplifica la administración y mejora la conectividad.

## Conclusión

Los switches multilayer son una herramienta esencial en la construcción de redes modernas, ofreciendo tanto funcionalidad como flexibilidad. Su capacidad para realizar tanto switching como routing en un solo dispositivo los convierte en una elección popular para empresas que buscan optimizar su infraestructura de red.

### [Realización Práctica](EjemploMls.md)