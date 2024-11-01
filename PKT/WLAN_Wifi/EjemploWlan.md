## A. WLAN Wifi usando autenticación en Access Points

- Configura el Access Point (AP):
    - Establece el método de autenticación en `WPA2-PSK`.
    - Configura la clave compartida (por ejemplo, `12345678`).
    - Usa encriptación `AES`.
    - Conecta el AP a un Switch que distribuya la conexión cableada.

- Configura el terminal de usuario:
    - Si usas un portátil, instala una tarjeta de red WiFi (`PT-LAPTOP-NM-1W-AC`) en la configuración física.
    - Configura la interfaz `Wireless0` con la misma clave compartida (`12345678`) y encriptación `AES`.
    - Activa la configuración de dirección IP y el gateway por DHCP para que obtenga una dirección IP automáticamente.

- Prueba la conexión:
    - Verifica que el terminal de usuario se conecte y obtenga una IP del servidor DHCP.
    - El usuario debería poder cambiar entre APs y mantener su dirección IP.

---

## B. WLAN Wifi usando Router Wifi y autenticación en servidor RADIUS

- Configura el servidor RADIUS:
    - Establece la IP de `192.168.1.2/24` en la interfaz `Fa0`.
    - Activa el servicio AAA (RADIUS) y añade un cliente RADIUS (el router WRT300N), con IP `192.168.1.3` y clave `pass`.
    - Añade usuarios en la sección de configuración de usuarios, por ejemplo, `user1/user1` y `user2/user2`.

- Configura el router WRT300N:
    - En la interfaz `Internet`, asigna la IP `192.168.1.3/24`.
    - En la interfaz `LAN`, asigna la IP `192.168.0.1/24` y actívalo como servidor DHCP.
    - En la interfaz Wireless, usa autenticación `WPA2-Enterprise` e ingresa la IP del servidor RADIUS (`192.168.1.2`), clave `pass`, y encriptación `AES`.

- Configura los terminales de usuario:
    - Conecta los dispositivos a la red WiFi configurada en el router.
    - Establece autenticación `WPA2` con los usuarios configurados en el servidor RADIUS (`user1/user1` o `user2/user2`).
    - Configura DHCP para obtener una IP automáticamente.

- Prueba la conexión:
    - Asegúrate de que los dispositivos pueden autenticarse correctamente y obtener IPs del servidor DHCP.

---

## C. WLAN Wifi usando Wireless LAN Controller, Light Weight Access Points y autenticación en servidor RADIUS

- Configura en el servidor Radius-DHCP:
   - Configura la interfaz `Fa0` (`192.168.100.2`) y el Default Gateway (`192.168.100.1`).
   - Activa y configura también el servicio DHCP con 3 pools:
     - **Pool para la gestión de la red:** `192.168.0.0/24`, IP inicial `192.168.0.4`, gateway `192.168.0.1`, usuarios máximos `251`, dirección WLC `192.168.200.3`.
     - **Pool para la red Wifi en VLAN 20:** `192.168.20.0/24`, IP inicial `192.168.20.4`, gateway `192.168.20.1`, usuarios máximos `251`, dirección WLC `192.168.200.3`.
     - **Pool para la red Wifi en VLAN 30:** `192.168.30.0/24`, IP inicial `192.168.30.4`, gateway `192.168.30.1`, usuarios máximos `251`, dirección WLC `192.168.200.3`.
   - Configura el servicio AAA (Radius):
     - En la sección 'Network Configuration', añade el cliente Radius (dispositivo desde el cual llegan peticiones de autenticación Radius), en este caso el WLC.
     - Asigna al cliente Radius un nombre (por ejemplo, `wlc0`), IP `192.168.200.3` y clave compartida `pass`.
     - En 'User Setup', configura los nombres de usuario y contraseñas de los usuarios, por ejemplo, `user3/user3` y `user4/user4`.

- Configura el Router0:
   - Verifica que el Router0 tiene configuradas las IPs en todas las interfaces y subinterfaces, así como el VLAN ID en las subinterfaces.
   - Configura DHCP relay en las interfaces `gi0/0`, `gi0/0.20` y `gi0/0.30` para reenviar las peticiones DHCP al servidor DHCP. (`ip helper-address 192.168.100.2`)

- Configura el WLC (Wireless LAN Controller):
   - Configura el direccionamiento IP en la interfaz de gestión. Ip address (`192.168.200.3`), Gateway (`192.168.200.1`)
   - En el WLC, puedes configurar grupos de LAPs y una o varias redes Wifi, asociando los grupos de LAPs a las redes Wifi:
     - Si se usa una única red Wifi, puede configurarse en VLAN 0 (indicando que no se usará etiqueta de VLAN 802.1q).
     - Con varias redes Wifi, cada WLAN puede usar una VLAN en los LAPs. El tráfico de control entre el WLC y los LAPs usará la VLAN nativa (por defecto, VLAN 1).
   - Configura dos redes Wifi compartiendo el mismo grupo de LAPs:
     - En 'Wireless LANs', configura el VLAN ID 20, nombre y SSID (por ejemplo, `wlan20` en ambos).
     - Selecciona autenticación WPA2 y en la configuración del servidor Radius, ingresa la IP `192.168.100.2`, clave `pass` y encriptación `AES`.
     - En 'Central Control', selecciona 'Local switching, local authentication' para que los LAPs manejen la autenticación de usuarios y conmutación de paquetes sin necesidad de redirigir todo el tráfico al WLC.
     - Haz clic en 'Save' para guardar los cambios.
     - Pulsa en 'New' y configura la segunda WLAN con VLAN ID 30 y SSID `wlan30`, repitiendo el resto de parámetros de configuración.
     - Pulsa 'Save' para guardar los cambios.

- Configuración de DHCP en el WLC:
   - En 'Config > DHCP', desactiva el servicio DHCP si no lo está ya.

- Configuración de los LAPs:
   - En la pestaña 'Physical', proporciona alimentación a los LAPs.
   - Activa la configuración de IP por DHCP en la sección 'Settings'.

- Configuración de los terminales de usuario:
   - Añade terminales inalámbricos en Packet Tracer.
   - Configura la interfaz Wireless del terminal con el SSID deseado (`wlan20` o `wlan30`), autenticación WPA2-Enterprise, usuario y contraseña configurados en el servidor Radius, encriptación AES y configuración de IP por DHCP.

Los terminales deberían adquirir una IP correspondiente a la WLAN elegida y tener conectividad entre sí y con el servidor.
