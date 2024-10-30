## Configuración de un Router con ACLs para control de tráfico

Este documento describe la configuración de un router utilizando ACLs (Listas de Control de Acceso) para filtrar el tráfico de red entre diferentes segmentos. 

**1. Configuración de Interfaces**

Primero, se configura la dirección IP de cada interfaz del router:

```
enable
configure terminal

interface GigabitEthernet0/0
  ip address 192.168.200.1 255.255.255.0
  no shutdown
exit

interface GigabitEthernet0/1
  ip address 200.0.0.1 255.255.255.0
  no shutdown
exit

interface GigabitEthernet0/2
  ip address 220.0.0.1 255.255.255.0
  no shutdown
exit
```

**2. Configuración de ACLs para la Red Interna**

Se define la ACL **100** para la red interna (192.168.200.0/24):

**Reglas de Salida (ACL 100):**

```
access-list 100 permit tcp 192.168.200.0 0.0.0.255 any established  
access-list 100 permit icmp any 192.168.200.0 0.0.0.255 echo-reply 
access-list 100 permit icmp any 192.168.200.0 0.0.0.255 unreachable
```

* Permite respuestas TCP de sesiones establecidas desde la red interna.
* Permite ICMP echo reply.
* Permite ICMP unreachable.

**Reglas de Entrada (ACL 100):**

```
access-list 100 permit ip 192.168.200.0 0.0.0.255 any
```

* Permite tráfico solo si tiene IP origen de la red interna.

**3. Configuración de ACLs para la Red Web**

Se define la ACL **101** para la red web (200.0.0.0/24), donde está el servidor web:

**Reglas de Salida (ACL 101):**

```
access-list 101 permit tcp 192.168.200.0 0.0.0.255 host 200.0.0.10 eq 80 
access-list 101 permit icmp 192.168.200.0 0.0.0.255 200.0.0.0 0.0.0.255 echo
```

* Permite tráfico HTTP de la red interna hacia el servidor web (200.0.0.10).
* Permite tráfico ICMP con origen en la red interna.

**Reglas de Entrada (ACL 101):**

```
access-list 101 permit ip 200.0.0.0 0.0.0.255 any
```

* Permite tráfico solo si tiene IP origen de la red web.

**4. Configuración de ACLs para el Filtro de Internet**

Se define la ACL **102** para el tráfico de Internet (220.0.0.0/24):

**Reglas de Salida (ACL 102):**

```
access-list 102 permit ip 192.168.200.0 0.0.0.255 any 
access-list 102 permit tcp host 200.0.0.10 eq www any established
```

* Permite tráfico de origen en la red interna.
* Permite respuestas procedentes del servidor web de peticiones iniciadas desde la red interna.

**Reglas de Entrada (ACL 102):**

```
access-list 102 deny ip 192.168.200.0 0.0.0.255 any
access-list 102 deny ip 200.0.0.0 0.0.0.255 any
access-list 102 deny ip 127.0.0.0 0.255.255.255 any
access-list 102 permit tcp any host 200.0.0.10 eq www
access-list 102 permit tcp any 192.168.200.0 0.0.0.255 established
access-list 102 permit icmp any 192.168.200.0 0.0.0.255 echo-reply
access-list 102 permit icmp any 192.168.200.0 0.0.0.255 unreachable
```

* **Denegar** tráfico de IPs de la red interna, el servidor web, y 127.0.0.0/8.
* **Permitir** peticiones web al servidor web.
* **Permitir** respuestas TCP hacia la red interna.
* **Permitir** ICMP echo reply hacia la red interna.
* **Permitir** ICMP unreachable hacia la red interna.

**5. Asignación de las ACLs a las Interfaces**

Se aplica la ACL correspondiente a cada interfaz:

```
interface GigabitEthernet0/0
  ip access-group 100 in
  ip access-group 100 out
exit

interface GigabitEthernet0/1
  ip access-group 101 in
  ip access-group 101 out
exit

interface GigabitEthernet0/2
  ip access-group 102 in
  ip access-group 102 out
exit
```

**Resumen de Filtros:**

* **Filtro Red Interna (ACL 100):** Controla el tráfico interno, permitiendo respuestas y ciertas restricciones de ICMP.
* **Filtro Red Web (ACL 101):** Controla el acceso al servidor web, permitiendo solo tráfico HTTP y ICMP permitido.
* **Filtro Internet (ACL 102):** Controla el acceso desde/hacia Internet, restringiendo IPs internas y permitiendo solo ciertos tipos de tráfico seguro.

**Nota:** Este es un ejemplo básico. La configuración de ACLs puede variar según las necesidades de seguridad de la red.