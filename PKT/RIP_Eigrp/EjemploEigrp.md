
## Configuración EIGRP
### Ejemplo configuración switch 3

```bash
Router(config)# router eigrp 1
Router(config-router)# network 192.168.0.0
Router(config-router)# network 192.168.1.0
Router(config-router)# no auto-summary
```

#### Explicación de los comandos:
- **router eigrp 1**: Este comando habilita EIGRP (Enhanced Interior Gateway Routing Protocol) en el router y lo asocia con el Autonomous System (AS) número `1`. EIGRP es un protocolo de enrutamiento avanzado utilizado para compartir información de rutas de manera eficiente.
- **network [IP de red]**: Configura las redes directamente conectadas que deben ser incluidas en el proceso de enrutamiento EIGRP. En este caso, se están anunciando las redes `192.168.0.0` y `192.168.1.0` a EIGRP.
- **no auto-summary**: Desactiva la función de resumen automático de rutas en EIGRP. Por defecto, EIGRP realiza un resumen automático de las redes en límites de clase, pero con este comando, se permite una mayor flexibilidad para anunciar rutas específicas.

### Ejemplo configuración router 7

```bash
Router(config)# router eigrp 1
Router(config-router)# network 192.168.4.0
Router(config-router)# passive-interface gi0/0
Router(config-router)# redistribute static metric 100000 1000 255 1 1500
Router(config-router)# exit
Router(config)# ip route 0.0.0.0 0.0.0.0 200.0.0.2
```

#### Explicación de los comandos:
- **router eigrp 1**: Habilita EIGRP en el router y lo asocia con el AS número `1`.
- **network 192.168.4.0**: Anuncia la red `192.168.4.0` como parte del proceso de enrutamiento EIGRP.
- **passive-interface gi0/0**: Configura la interfaz `GigabitEthernet0/0` para que no envíe actualizaciones de enrutamiento EIGRP, aunque aún puede recibirlas.
- **redistribute static metric 100000 1000 255 1 1500**: Redistribuye las rutas estáticas en el proceso EIGRP con una métrica específica. Los parámetros representan:
  - `100000`: La banda ancha en Kbps.
  - `1000`: El retardo en microsegundos.
  - `255`: La fiabilidad de la ruta.
  - `1`: El valor mínimo de carga de la ruta.
  - `1500`: El tamaño máximo de paquete de la ruta.
- **ip route 0.0.0.0 0.0.0.0 200.0.0.2**: Configura una ruta estática por defecto, indicando que todo el tráfico no especificado debe enviarse a `200.0.0.2`, que sería la puerta de enlace.

### Sumarización

La sumarización en EIGRP permite reducir la cantidad de rutas enviadas entre routers al agrupar varias redes en una sola red. Esto ayuda a optimizar el uso del ancho de banda y mejorar la eficiencia del enrutamiento.

#### Sumarización switch 3
```bash
Switch(config)# interface gi1/0/1
Switch(config-if)# ip summary-address eigrp 1 10.0.0.0 255.255.0.0
Switch(config)# interface gi1/0/2
Switch(config-if)# ip summary-address eigrp 1 10.0.0.0 255.255.0.0
```

#### Explicación de los comandos:
- **ip summary-address eigrp 1 [IP de red] [máscara de subred]**: Configura una dirección de resumen para EIGRP en una interfaz, lo que significa que las rutas más específicas se agruparán en una sola red. En este caso, `10.0.0.0 255.255.0.0` representa un resumen de varias redes dentro del rango `10.0.0.0/16`.

#### Sumarización switch 4
```bash
Switch(config)# interface gi1/0/1
Switch(config-if)# ip summary-address eigrp 1 10.1.0.0 255.255.0.0
Switch(config)# interface gi1/0/2
Switch(config-if)# ip summary-address eigrp 1 10.0.0.0 255.255.0.0
```

#### Explicación de los comandos:
- **ip summary-address eigrp 1 [IP de red] [máscara de subred]**: Realiza el mismo proceso de sumarización, pero con diferentes direcciones de red, en este caso `10.1.0.0 255.255.0.0` y `10.0.0.0 255.255.0.0`.