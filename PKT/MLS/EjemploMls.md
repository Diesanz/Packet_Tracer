## Configuración de Switches Multicapa

Este archivo .md describe la configuración básica de switches multicapa (layer 3) en Cisco IOS.

### 1. Configurar puertos en modo access/trunk

Para configurar un puerto en modo **access**, se debe especificar la VLAN a la que pertenece:

```
(config)# interface [puerto]
(config-if)# switchport mode access
(config-if)# switchport access vlan [VLAN_ID]
```

Para configurar un puerto en modo **trunk**, se debe especificar las VLANs que se permitirán:

```
(config)# interface [puerto]
(config-if)# switchport mode trunk
(config-if)# switchport trunk allowed vlan [VLAN_ID_list]
```

* En este ejemplo solo las interfaces conectadas a los PCs estarán en modo access

### 2. Configurar VTP y VLANs

Para configurar VTP, se necesitan los siguientes comandos:

```
(config)# vtp domain [nombre_dominio]
(config)# vtp mode [modo_VTP]
```
* Multilayer Switch 0 y 1 en modo server, el resto modo client

Para crear una VLAN, se usa el siguiente comando (ejecutar solo en Multilayer Switch 0):

```
(config)# vlan [VLAN_ID]
(config-vlan)# name [nombre_VLAN]
```

### 3. Configurar PVST

Para activar PVST, se usa el siguiente comando:

```
(config)# spanning-tree vlan [vlan-id] root (primary/secondary)
```


### 4. Activar IP Routing

Para activar IP routing en un switch multicapa, se usa el siguiente comando:

```
(config)# ip routing
```

### 5. Configurar dirección IP y HSRP
En un multilayer switch la config de IP de VLANes no va en subinterfaces, como en los routers, sino en interfaces virtuales llamadas "svi" (switched virtual interfaces). Estas interfaces corresponden con la VLAN: interface vlan 30.

Para configurar una interfaz VLAN (SVI), se usa el siguiente comando:

```
(config)# interface vlan [VLAN_ID]
```

Luego, se configura la dirección IP y HSRP:

```
(config-if)# ip address [dirección_IP] [máscara]
(config-if)# standby [grupo_HSRP] ip [dirección_virtual]
(config-if)# standby [grupo_HSRP] priority [prioridad]
```

Ejemplo completo de la vlan 30 en Multilayer Switch 0 

```
interface vlan 30
 ip address 192.168.30.2 255.255.255.0
 standby 10 ip 192.168.30.1
 standby 10 priority 110
 standby 10 preempt
 standby 10 track GigabitEthernet 1/0/3
```

- Salida comando `show standby brief en Multilayer Switch 0 `
    ![alt text](image.png)
- Salida comando `show standby brief en Multilayer Switch 1 `
    ![alt text](image-1.png)

### 6. Configurar interfaces IP conectadas al router

Para configurar una interfaz IP conectada al router, se debe desactivar el switching y configurar la dirección IP:

```
(config)# interface [puerto]
(config-if)# no switchport
(config-if)# ip address [dirección_IP] [máscara]
```

### 7. Configurar routing dinámico

Recuerde que en RIP debe configurar en cada router únicamente las redes directamente conectadas que quiera que RIP anuncie a otros routers.

- Multilayer Switch 0

    ![alt text](image-2.png)
- Multilayer Switch 0

    ![alt text](image-3.png)
- To_ISP

    ![alt text](image-4.png)

A continuación, realice en To_ISP mediante CLI la siguiente configuración.
```
to_isp#configure terminal
to_isp(config)#router rip
to_isp(config-router)#default-information originate
to_isp(config-router)#exit
to_isp(config)#exit
to_isp#copy running-config startup-config
```
### 8. Sumarizar anuncios de rutas

Para sumarizar anuncios de rutas en RIP, se debe configurar la red como una red de clase.

**Nota:** Este ejemplo es una guía básica. Se pueden agregar más configuraciones según las necesidades del entorno.