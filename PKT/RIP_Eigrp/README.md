
# Introducción a RIP y EIGRP

## RIP (Routing Information Protocol)

RIP es un protocolo de enrutamiento dinámico utilizado para intercambiar información de rutas entre routers en una red. Usa el número de saltos como métrica para determinar la mejor ruta, con un máximo de 15 saltos, lo cual limita su uso en redes pequeñas o medianas.

### Versiones de RIP

**RIPv1:**

* Es la versión más antigua y es **classful**, lo que significa que no soporta máscaras de subred, limitando su uso en redes con subredes no contiguas.
* Solo puede trabajar con clases de red por defecto, como 10.0.0.0 para la clase A, lo que puede llevar a problemas de enrutamiento en redes más complejas.

**RIPv2:**

* Es una versión mejorada y **classless**, lo que le permite soportar máscaras de subred y enrutar subredes no contiguas.
* Incluye campos adicionales en los mensajes para manejar máscaras de subred y proporciona autenticación para mayor seguridad.

### Configuración de RIP en dispositivos Cisco

**RIPv1:**

```bash
Router(config)# router rip
Router(config-router)# network [red_directamente_conectada]
```

**RIPv2:**

```bash
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# no auto-summary
Router(config-router)# network [red_directamente_conectada]
```

### Comandos útiles en RIP

* **Ver tabla de rutas:**

```bash
Switch# show ip route
```

* **Activar y desactivar debug de RIP:**

```bash
Switch# debug ip rip
Switch# undebug all
```
### [Realización Práctica](EjemploRip.md)

___

## EIGRP (Enhanced Interior Gateway Routing Protocol)

EIGRP es un protocolo de enrutamiento avanzado desarrollado por Cisco, que utiliza una combinación de métricas (ancho de banda, retardo, fiabilidad, carga y MTU) para seleccionar la mejor ruta. Es más eficiente que RIP y es adecuado para redes grandes y complejas.

### Características principales de EIGRP
* **Soporta balanceo de carga.**
* **Utiliza una métrica compuesta basada en varios factores.**
* **Mantiene una tabla de topología con todas las rutas conocidas, lo que le permite recuperarse rápidamente de fallas en la red.**
* **Es classless, permitiendo el uso de máscaras de subred.**

### Configuración básica de EIGRP
#### Configurar EIGRP en un sistema autónomo:
```bash
Router(config)# router eigrp [AS]
Router(config-router)# network [red_directamente_conectada]
Router(config-router)# no auto-summary
Router(config-router)# passive-interface [interfaz]
```

#### Configuración de una ruta por defecto en EIGRP:
```bash
Router(config)# ip route 0.0.0.0 0.0.0.0 [IP_o_interfaz_destino]
Router(config)# router eigrp [AS]
Router(config-router)# redistribute static metric 100000 1000 255 1 1500
```

#### Sumarización de rutas en EIGRP:
```bash
Switch(config)# interface gi1/0/1
Switch(config-if)# ip summary-address eigrp [AS] [red_sumarizada] [máscara]
```

### Comandos útiles en EIGRP
* **Ver tabla de rutas y estado de vecinos en EIGRP:**
```bash
Switch# show ip route
Switch# show ip eigrp neighbors
Switch# show ip eigrp topology
```
### [Realización Práctica](EjemploEigrp.md)