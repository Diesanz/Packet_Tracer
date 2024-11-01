# Redes Wlan

## Presentación

En esta práctica se exploran diversas formas de configurar redes inalámbricas (WLAN) usando Wi-Fi en un entorno simulado con Cisco Packet Tracer. Se aborda la configuración de dispositivos de red que permiten implementar seguridad y control en redes Wi-Fi mediante distintos métodos de autenticación y diferentes infraestructuras de gestión de puntos de acceso. Este documento proporciona un recorrido por tres configuraciones de WLAN, cada una con sus características y requerimientos específicos.

### Conceptos Clave

- __WLAN y Wi-Fi__

    Las redes WLAN (Wireless Local Area Network) permiten la comunicación entre dispositivos dentro de una área local sin necesidad de cables, utilizando señales de radiofrecuencia a través de dispositivos de Wi-Fi. Estas redes se utilizan tanto en entornos domésticos como empresariales para proporcionar conectividad móvil a dispositivos como teléfonos, tablets y portátiles.

- __Autenticación en Redes Inalámbricas__

    Para proteger el acceso a redes inalámbricas, se emplean diversos métodos de autenticación. En esta práctica, se configuran diferentes métodos de autenticación en redes WLAN:

    - Autenticación en Access Points (APs): Se usa una clave compartida configurada en el punto de acceso (WPA2-PSK) para autenticación básica.
   -  Autenticación con servidor Radius: Permite una autenticación centralizada y segura para cada usuario a través de un servidor Radius (WPA2-Enterprise) utilizando el protocolo 802.1X.
   -  Controlador de LAN inalámbrica (WLC) con puntos de acceso ligeros (LAPs): Permite la administración centralizada de múltiples puntos de acceso mediante un WLC, ideal para redes grandes con múltiples usuarios y APs.

- __Protocolo 802.1X y Radius__

    El protocolo IEEE 802.1X permite el control de acceso a la red, obligando a los dispositivos conectados a autenticarse a través de un servidor externo, generalmente mediante el protocolo Radius. Esto permite una autenticación por usuario más segura y controlada en comparación con métodos de clave compartida, ya que cada usuario tiene su propio nombre de usuario y contraseña.

- __Controlador de LAN Inalámbrica (WLC) y Puntos de Acceso Livianos (LAP)__

    El WLC (Wireless LAN Controller) permite la administración de múltiples puntos de acceso desde un solo controlador, lo cual facilita la operación y mantenimiento de redes grandes. Los LAPs (Light Weight Access Points) son puntos de acceso controlados por el WLC, lo que permite una experiencia de roaming fluida y la implementación de redes con seguridad avanzada, mediante autenticación delegada en un servidor Radius.

- __Protocolo CAPWAP y LWAPP__

    Estos protocolos permiten la comunicación segura entre el WLC y los LAPs. CAPWAP, especificado en la RFC 5416, es un protocolo de encapsulamiento que asegura el tráfico de control entre el controlador y los puntos de acceso.

### [Realización Práctica](EjemploWlan.md)