
## Configuración RIP

1. **Rip versión 1**

    Antes de configurar RIP en un router, debes asegurarte de que el enrutamiento esté habilitado. Esto lo haces con el siguiente comando: `ip routing`. Este comando habilita el proceso de enrutamiento en el router. Es necesario para que el router pueda procesar y distribuir las rutas a través de protocolos como RIP.

    ### Ejemplo configuración switch 3

    ```bash
    Router(config)# router rip
    Router(config-router)# network 192.168.1.0
    Router(config-router)# network 192.168.2.0
    Router(config-router)# network 10.0.10.0
    Router(config-router)# network 10.0.20.0
    ```
    * **router rip**: Este comando configura el router para usar el protocolo RIP. Se entra en el modo de configuración de RIP.

    * **network [IP de red]**: Con este comando, especificamos las redes directamente conectadas al router que deben ser anunciadas a través de RIP. Cada red debe estar conectada al router de alguna forma (por ejemplo, una interfaz o subinterfaz de una VLAN).
    
    ### Ejemplo configuración switch 2

    ```bash
    Router(config)# router rip
    Router(config-router)# network 192.168.0.0
    Router(config-router)# network 192.168.2.0
    ```

    ### Ejemplo configuración router 7

    ```bash
    Router(config)# router rip
    Router(config-router)# network 192.168.4.0
    Router(config-router)# default-information originate
    Router(config-router)# exit
    Router(config)# ip route 0.0.0.0 0.0.0.0 200.0.0.2
    Router(config)# router rip
    Router(config-router)# passive-interface gi0/0
    ```

    * **network 192.168.4.0:**  Se anuncia la red 192.168.4.0 en RIP. Esto permite que otros routers que ejecutan RIP en la misma red aprendan sobre la existencia de esta red y puedan enrutar tráfico hacia ella.

    * **default-information originate:** Este comando indica al router que también debe anunciar la ruta por defecto (0.0.0.0). Esta ruta por defecto se utiliza para enrutar tráfico a cualquier destino que no esté específicamente configurado en la tabla de enrutamiento. Esto es útil si se desea que los routers en la red utilicen una puerta de enlace por defecto.

    * **exit:** Sale del modo de configuración de RIP.

    * **ip route 0.0.0.0 0.0.0.0 200.0.0.2:** Configura una ruta estática por defecto. Este comando indica al router que cualquier tráfico que no tenga una ruta específica debe ser enviado a la dirección 200.0.0.2. Esta dirección es la puerta de enlace por defecto para la red.

    * **passive-interface gi0/0:** Este comando configura la interfaz GigabitEthernet0/0 para que no envíe actualizaciones RIP, pero aún así reciba las actualizaciones. Esto es útil cuando no se quiere que ciertas interfaces participen activamente en el intercambio de rutas, como las interfaces de acceso o las que están conectadas a redes no RIP. 

    **Explicación:**

    La configuración anterior define un router que utiliza RIP para anunciar su red y una ruta por defecto. También configura una ruta estática por defecto para el tráfico no especificado. Además, la interfaz GigabitEthernet0/0 está configurada como pasiva para evitar que participe activamente en el intercambio de rutas RIP.

    **Nota:**

    La configuración de la ruta por defecto y la interfaz pasiva depende de los requisitos específicos de la red. Es importante analizar cuidadosamente las necesidades de enrutamiento y seguridad antes de implementar estas configuraciones.

2. **Rip versión 2**

    Configuración para router y multilayerswitches
    
    ```bash
    Router(config)# router rip
    Router(config-router)# version 2
    Router(config-router)# no auto-summary  
    ```

    * **version2**:  Este comando cambia la versión de RIP a la versión 2, que soporta máscaras de subred de longitud variable (VLSM) y otras características avanzadas.

    * **no auto-summary**: Desactiva la función de resumen automático de subredes en RIP. En RIP v1, las subredes se resumen automáticamente a la clase de red más cercana, lo que puede ser problemático cuando se usan subredes no contiguas. Este comando desactiva ese comportamiento, permitiendo un mayor control sobre las rutas.

    Una vez hecho esto realizar comandos en switch 3 y router 0:

    ```bash
    Router(config)# router rip
    Router(config-router)# passive-interface vlan10
    Router(config-router)# passive-interface vlan20
    ```

    ```bash
    Router(config)# router rip
    Router(config-router)# passive-interface gi0/0.110
    Router(config-router)# passive-interface gi0/0.120
    ```

    * **passive-interface vlan10 y passive-interface vlan20** : Estos comandos configuran las interfaces VLAN 10 y VLAN 20 para que no envíen actualizaciones RIP, pero todavía pueden recibirlas. Esto es útil cuando no se necesita intercambiar rutas a través de esas interfaces.

    Recordar que antes de esto hay que configurar las interfaces en subinterfaces:

    ```bash
    Router(config)# interface GigabitEthernet0/0.110
    Router(config-subif)# encapsulation dot1Q 110
    Router(config-subif)# ip address 10.1.110.1 255.255.255.0
    ```

