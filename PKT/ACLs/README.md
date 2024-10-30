# Listas ACLs

## Presentación
Las listas de control de acceso (ACL) son conjuntos de condiciones que regulan el tráfico en las interfaces de un router, actuando como un firewall para permitir o denegar paquetes según criterios específicos. Estas listas ayudan a gestionar el tráfico de red, mejorando el rendimiento al limitar el flujo de datos no deseados y proporcionando seguridad al restringir el acceso a diferentes áreas de la red.

___Funciones principales de las ACL:___
- Filtrado de tráfico: Controlan qué paquetes se envían o bloquean, basándose en direcciones IP, protocolos y números de puerto.
- Mejora del rendimiento: Al restringir ciertos tipos de tráfico, como video o actualizaciones de enrutamiento, se reduce la carga en la red.
- Seguridad: Permiten que solo ciertos dispositivos accedan a áreas sensibles de la red.
- Control de acceso: Administran qué usuarios pueden acceder a determinados recursos, como archivos o servicios.

___Consideraciones en la configuración de ACL:___
- Las sentencias se procesan secuencialmente desde el inicio hasta el final. Al encontrar una coincidencia, se toma la acción correspondiente (permitir o denegar) y no se evalúan más condiciones.
- Existe una sentencia implícita "deny any" al final de cada ACL, que rechaza cualquier paquete que no coincida con las condiciones anteriores.
- Las ACL se deben aplicar en el lugar adecuado: las estándar cerca del destino y las extendidas cerca del origen.

___Sintaxis:___

**ACL Estándar:**
```
Router(config)access-list access-list-number {deny | permit | remark} source [source-wildcard] [log]
```
**ACL Extendida:**
```
Router(config)# access-list access-list-number {deny | permit | remark} {tcp | udp | icmp} source [source-wildcard] [destination [destination-wildcard]] [log]
```
Las máscaras de wildcard se utilizan para definir las direcciones IP y pueden representar rangos de direcciones. Las palabras clave "any" y "host" permiten especificar direcciones de origen y destino de manera más sencilla.

## Aplicación ACLs

Las listas de control de acceso (ACL) se pueden enlazar a una interfaz utilizando el comando `ip access-group`, que permite aplicar ACLs extendidas a una interfaz específica. Es importante recordar que solo se pueden asignar dos ACL por interfaz y protocolo: una para el tráfico entrante (in) y otra para el saliente (out). La sintaxis del comando es la siguiente:

```
Router(config-if)# ip access-group access-list-number {in | out}
```

__Consideraciones importantes__
- Una ACL con sentencias numeradas no se puede modificar directamente; debe ser eliminada con no access-list list-number y luego recreada.

__Mostrar información sobre las ACL__
- El comando `show ip interface` proporciona detalles sobre las interfaces IP y cualquier ACL aplicada.
- `show access-lists` muestra el contenido de todas las ACL en el router. Para ver una lista específica, se puede añadir el número o nombre de la ACL.
- `show running-config` también revela las ACL y su configuración.

## ACLs típicas

**Permitir respuestas a peticiones TCP:**

```
access-list 101 permit tcp any [source_network] [wildcard_mask] established
```

**Permitir pings desde una red:**

```
access-list 101 permit icmp [source_network] [wildcard_mask] any echo
```

**Permitir pings hacia un host específico:**

```
access-list 101 permit icmp [source_network] [wildcard_mask] host [destination_IP]
```

**Permitir paquetes HTTP a un host destino:**

```
access-list 101 permit tcp any host [destination_IP] eq www
```

**Denegar tráfico desde una red:**

```
access-list 101 deny ip [source_network] [wildcard_mask] any
```


### [Realización Práctica](EjemploACL.md)