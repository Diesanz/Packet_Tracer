# DNS y NAT

## Presentación

DNS se refiere a toda la infraestructura que existe en Internet para gestionar los nombres de dominio. Los nombres de dominio son asociados a direcciones IP que facilitan su manejo. Los dominios pueden tener subdominios, que permiten agrupar conjuntos de máquinas de forma conveniente. El nombre de dominio, junto con el nombre del host, crea lo que se denomina **Fully Qualified Domain Name (FQDN)**. Por ejemplo, `subred.mired.es` es un subdominio del dominio `mired.es`, dentro del cual puede encontrarse la máquina `xyz.subred.mired.es` con la dirección IP `123.123.123.123`.

![alt text](/DNS_NAT/imagenes/image.png)

## Funcionamiento Básico
Aunque la configuración del servicio DNS está fuera del alcance de esta asignatura, es conveniente comprender su funcionamiento básico para ubicar correctamente los servidores DNS. DNS actúa de forma similar a las agendas de los teléfonos modernos: en lugar de teclear el número de un abonado, se elige un nombre y la agenda se encarga de hacer la traducción necesaria.

## Funciones Adicionales de DNS
Además del servicio básico de traducción de nombres a direcciones IP, DNS proporciona otros servicios, tales como:

- **Alias de Host**: Permite que una máquina sea conocida por varios nombres.
- **Alias de Servidor de Correo**: Permite averiguar la dirección del servidor de correo para un dominio.
- **Distribución de Carga**: Permite que varias direcciones IP estén asociadas al mismo nombre de máquina.

## Estructura de DNS
DNS está compuesto por una base de datos distribuida, organizada en una jerarquía de servidores de nombres y un protocolo para consultar esa base de datos. La jerarquía de DNS se agrupa en diferentes niveles. 

### Niveles de DNS
- **Nivel Raíz (Root)**: Guarda la base de datos de servidores DNS autoritativos de dominios del siguiente nivel, llamado **Top-Level Domains (TLD)**.
- **Dominios de Segundo Nivel**: Pueden contener subdominios y hosts.

## Espacios de Nombres
En una organización, habrá un espacio de nombres expuesto al exterior y un espacio de nombres de uso interno. El espacio de nombres externo es utilizado por usuarios externos para acceder a servicios públicos, mientras que el interno es utilizado por usuarios de la red para acceder a servicios internos. Generalmente, se utilizan DNS distintos para cada espacio de nombres.

## Tipos de DNS
Dentro de la jerarquía de DNS, se establecen zonas para las cuales hay servidores de nombres autoritativos. Al adquirir un dominio, se debe asociar a dos direcciones IP de servidores de nombres autoritativos.

![alt text](/DNS_NAT/imagenes/image-1.png)

### Tipos de Servidores Autoritativos
1. **Primarios**: Contienen los ficheros de zona con la base de datos de nombres y pueden ser actualizados.
2. **Secundarios**: Contienen una copia de solo-lectura de los ficheros de zona, obtenida de los servidores primarios mediante un proceso llamado **Transferencia de Zona (Zone Transfer)**.

![alt text](/DNS_NAT/imagenes/image-2.png)
## Tipos de Registros DNS
Los registros almacenados en un servidor DNS pueden ser de distintos tipos. En esta práctica, utilizaremos solo registros de tipo A (host) que guardan una dirección IP asociada a un nombre de host, y registros de tipo NS que indican cuál es el servidor autoritativo de un dominio.

## Redundancia
Los sistemas DNS suelen ser críticos y conviene que estén redundados. Esto se puede lograr mediante la instalación de varios DNS secundarios, así como servidores de reenvío y de solo-caché. También se puede implementar alta disponibilidad mediante configuraciones que utilizan servidores replicados y sincronizados.

![alt text](/DNS_NAT/imagenes/image-3.png)

## Conclusión
El diseño y la gestión adecuada de un sistema DNS son fundamentales para garantizar la accesibilidad y la resiliencia de los servicios de red. Comprender los conceptos básicos y las funciones adicionales del DNS puede ayudar en la correcta implementación de esta infraestructura.

### [Realización Práctica](EjemploDNS_NAT.md)