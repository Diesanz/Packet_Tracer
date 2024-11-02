# Ejemplo de Configuración Sencilla de DNS y NAT en Packet Tracer
![alt text](/DNS_NAT/imagenes/image-4.png)
## Introducción

En esta práctica, realizaremos una configuración sencilla de DNS y NAT en Packet Tracer, teniendo en cuenta las limitaciones del programa. Trabajaremos con la red correspondiente al fichero de Packet Tracer disponible en el campus virtual.

## Objetivos

- Dividir una red en un dominio de nombres interno y externo.
- Configurar un servidor DNS autoritativo.
- Implementar NAT estático y NAT con sobrecarga (PAT).

## Configuración del Servidor DNS

### Servidor DNS en la DMZ

Configuraremos el servidor DNS ubicado en la DMZ, llamado `ns.www`, que será el servidor autoritativo del dominio `www`. Este servidor almacenará:

- Información del servidor autoritativo (él mismo).
- Información de los hosts en el dominio, en este caso, el servidor web `www`.

![alt text](image-5.png)

### Servidor DNS Interno

Configuraremos un servidor DNS interno que guardará registros de nombres internos de la corporación (por ejemplo, la impresora `mad1pr01`). Este servidor también almacenará información sobre el servidor de nombres autoritativo del dominio `www` (`ns.www`).

![alt text](image-6.png)

### Servidor DNS Externo

Por último, configuraremos un servidor DNS externo, ubicado en Internet, que almacenará la información del servidor autoritativo del dominio `www`.

![alt text](image-7.png)

## Verificación del Funcionamiento

Podemos comprobar el funcionamiento activando el modo simulación y filtrando los protocolos DNS, HTTP e ICMP. Probaremos a acceder a la web `www` desde un host interno y otro externo, y haremos ping desde un host interno al nombre `mad1pr01`.

## Traducción de Direcciones de Red (NAT)

NAT es la traducción de direcciones IP de origen o destino en la cabecera IP. Un dispositivo que realiza NAT (gateway NAT) guarda una tabla de traducciones y reemplaza las direcciones IP en los paquetes. Existen diferentes tipos de NAT:

- **NAT estático**: mapeo uno a uno de direcciones IP.
- **NAT dinámico**: mapeo dinámico entre un pool de direcciones IP.
- **NAT con sobrecarga (PAT)**: traducción de IP y puerto, útil para múltiples dispositivos internos.

### Configuración de NAT en la Red

1. **Cambio de IP del Servidor DNS Externo**:
   - Cambiar la IP del servidor DNS externo `ns.www` a la dirección IP pública.

   ![alt text](image-8.png)

2. **Configuración de NAT Estático para Servidores**:
   - Asociar direcciones IP internas con sus traducidas en el exterior.
   - Configurar las interfaces interna y externa.

   ```bash
   Router# conf t
   Router(config)# ip nat inside source static [ip_a_traducir_servidor_Web] [ip_traducida_servidor_Web]
   Router(config)# ip nat inside source static [ip_a_traducir_servidor_DNS] [ip_traducida_servidor_DNS]
   Router(config)# int [interfaz_interna]
   Router(config-if)# ip nat inside
   Router(config-if)# int [interfaz_externa]
   Router(config-if)# ip nat outside
    ```

3. Configuración de PAT para la Red Interna:
- Configurar un rango de direcciones IP internas a traducir mediante una ACL.
- Configurar un pool de direcciones IP externas.
- Configurar la traducción del rango anterior por el pool anterior.

    ```bash
    Router(config)# access-list [ID_ACL] permit [subred_interna] [wildcard_mask]
    Router(config)# ip nat pool [nombre_pool] [IP_traducida] [IP_traducida] netmask [máscara]
    Router(config)# ip nat inside source list [ID_ACL] pool [nombre_pool] overload
    Router(config)# int [interfaz_interna]
    Router(config-if)# ip nat inside
    ```
4. Comprobación configuración:
    - Comando `show ip nat translations`
    ![alt text](image-10.png)
    - Comando `show ip nat stadistics`
    ![alt text](image-9.png)
### Conclusiones
Con esta configuración, hemos implementado exitosamente DNS y NAT en Packet Tracer, y podemos verificar su funcionamiento mediante simulaciones. Es importante tener en cuenta las limitaciones del software y las implicaciones de NAT en redes complejas.