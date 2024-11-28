# Pasos para Configurar OSPF en una Red Modular Jerárquica

A continuación, se presentan los pasos detallados para configurar OSPF en una topología modular jerárquica. Siguiendo esta guía, podrás activar OSPF, establecer conectividad entre las áreas, y optimizar el enrutamiento en la red.

![alt text](/PKT/OSPF/imagenes/image4.png)

## Pasos Anteriores
1. Configuar cada router y switch con el comando `ip route`
2. Añadir las interfaces loopback:
```bash
Router(config)#interface loopback [número] -> 1
Router(config-if)#ip address [IP] [máscara] -> 10.0.0.1 255.255.255.255
``` 

## 1. Configurar interfaces Loopback

Asegúrate de que cada router que participará en OSPF tenga configurada una interfaz loopback con una dirección IP. Esta dirección será usada por OSPF para identificar cada router de manera única.  En esta práctica, se asume que las interfaces loopback ya están configuradas.


## 2. Activar OSPF en los routers

Entra en el modo de configuración de OSPF en cada router:

```bash
Router(config)#router ospf [pid]
```
Donde `[pid]` es el identificador del proceso OSPF (local al router).

Incluye en OSPF las redes directamente conectadas utilizando el comando `network` con la máscara wildcard y asignándolas a un área:

```bash
Router(config-router)#network [IP] [wildcard] area [area_id]
```
Ejemplo:

1. Switch 3 ![alt text](/PKT/OSPF/imagenes/image3.png)
2. Switch 1 ![alt text](/PKT/OSPF/imagenes/image-1.png)
3. Switch 2 ![alt text](/PKT/OSPF/imagenes/image-2.png)
4. Router 0 ![alt text](/PKT/OSPF/imagenes/image-3.png)
5. Router 7 ![alt text](/PKT/OSPF/imagenes/image-4.png)


## 3. Verificar las relaciones de vecinos OSPF

Comprueba que los routers hayan establecido relaciones de vecinos OSPF correctamente:

```bash
Router#show ip ospf neighbor
```
### 3.1 Verificar las relaciones de vecinos OSPF

Comprueba que los switches hayan establecido relaciones de vecinos OSPF correctamente:

![alt text](/PKT/OSPF/imagenes/image-5.png)

## 4. Configurar interfaces pasivas

Configura como pasivas las interfaces que no necesitan enviar mensajes OSPF, como las que están conectadas a redes externas (por ejemplo, a Internet):

```bash
Router(config-router)#passive-interface [interfaz]
```

## 5. Configurar una ruta por defecto y redistribuirla en OSPF

En el router de acceso a Internet, configura una ruta por defecto:

```bash
Router(config)#ip route 0.0.0.0 0.0.0.0 [IP_o_interfaz_destino]
```
![alt text](/PKT/OSPF/imagenes/image-6.png)

Redistribuye esta ruta por defecto en OSPF:

```bash
Router(config-router)#default-information originate
```

## 6. Configurar sumarización de rutas

### 6.1. Sumarización en sentido Core → Distribución

En el ABR del módulo de Internet, configura la sumarización hacia el área del módulo:

```bash
Router(config-router)#area 0 range [IP] [máscara]
```
Ejemplo: 

1. Switch 5 ![alt text](/PKT/OSPF/imagenes/image-7.png)


### 6.2. Configurar áreas Totally Stub

Para áreas sin conexión a redes externas, define el área como `totally stub`:

```bash
Router(config-router)#area [area_id] stub no-summary
```
Esto debe configurarse en todos los routers del área.

Ejemplo:

1. Router 6 ![alt text](/PKT/OSPF/imagenes/image-8.png)
2. Router 0 ![alt text](/PKT/OSPF/imagenes/image-9.png)

### 6.3. Sumarización en sentido Distribución → Core

En los ABRs de otras áreas, configura la sumarización hacia el área 0:

```bash
Router(config-router)#area [area_id] range [IP] [máscara]
```
Ejemplo: 

1. Switch 3 ![alt text](/PKT/OSPF/imagenes/image-10.png)
2. Router 6 ![alt text](/PKT/OSPF/imagenes/image-11.png)

## 7. Comprobación y troubleshooting

Realiza pings entre hosts para verificar conectividad.

Usa los siguientes comandos para inspeccionar la configuración y funcionamiento de OSPF:

```bash
Router#show ip route         # Ver tabla de rutas
Router#show ip protocols     # Ver protocolos de enrutamiento activos
Router#show ip ospf database # Ver base de datos OSPF
Router#show ip ospf neighbor # Ver lista de vecinos OSPF
```

Si los cambios no se reflejan correctamente, reinicia el proceso OSPF:

```bash
Router#clear ip ospf process
Reset ALL OSPF processes? [no]: yes
```

Si es necesario, guarda la configuración y reinicia el router:

```bash
Router#wr mem
Router#reload
```