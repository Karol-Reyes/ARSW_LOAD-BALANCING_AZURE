# Respuestas Parte 2 - Escalabilidad horizontal en Azure

## 1. Tipos de balanceadores, SKU e IP pública

**Tipos de balanceadores en Azure.** La diferencia principal es la capa en que trabajan y su alcance (regional o global):

| Servicio | Capa | Alcance | Para qué sirve |
|---|---|---|---|
| **Azure Load Balancer** | 4 (TCP/UDP) | Regional | Reparte tráfico entre VMs según IP y puerto. Es el que usa el laboratorio. |
| **Application Gateway** | 7 (HTTP/HTTPS) | Regional | Enruta por URL o cabeceras, termina SSL, incluye firewall de aplicaciones web (WAF) y afinidad por cookie. |
| **Traffic Manager** | DNS | Global | Responde consultas DNS con el endpoint más cercano o disponible, sin ver el tráfico. |
| **Front Door** | 7 | Global | Balanceo HTTP entre regiones, con CDN, WAF y aceleración. |

Dentro de Azure Load Balancer hay además dos variantes. El balanceador **público** tiene una IP pública y recibe tráfico de internet. El **interno** usa una IP privada y reparte tráfico entre capas dentro de una red virtual (por ejemplo, del frontend a la base de datos).

**¿Qué es un SKU?** Son las siglas de *Stock Keeping Unit*: el nivel o versión de producto que eliges para un recurso, que define sus capacidades, límites, SLA y precio. En Load Balancer y en IP pública hay dos:

| | Basic | Standard |
|---|---|---|
| Zonas de disponibilidad | No | Sí (zonal y zone-redundant) |
| Instancias en el back-end | Hasta 300 | Hasta 1000 |
| Sondeos de estado | HTTP y TCP | HTTP, HTTPS y TCP |
| Seguridad por defecto | Abierto | **Cerrado**: bloquea la entrada salvo que un NSG la permita |
| SLA | Ninguno | 99,99 % |

Microsoft retiró el SKU Basic en septiembre de 2025, así que hoy Standard es la única opción real. Además, un balanceador Standard solo se combina con IPs públicas Standard.

**¿Por qué necesita IP pública?** Porque es el punto de entrada único del sistema. Los clientes de internet necesitan una dirección a la cual enviar sus peticiones, y el balanceador las redirige a las IPs privadas de las VMs. Así el cliente nunca conoce cuántas VMs hay ni cuáles son, y se pueden agregar o quitar sin que cambie el endpoint. Un balanceador interno no la necesita, porque usa una IP privada.

## 2. Backend Pool

Es el conjunto de recursos que reciben el tráfico que reparte el balanceador: las interfaces de red (NIC) de las VMs, instancias de un Scale Set o direcciones IP. Es lo que hace posible la escalabilidad horizontal: para aumentar capacidad basta con agregar una VM al pool, y para reducirla, quitarla, sin tocar la IP pública ni la regla. En el laboratorio, `BackendPool` contiene a VM1, VM2 y VM3.

## 3. Health Probe

Es una comprobación periódica que el balanceador hace a cada máquina del pool para saber si puede atender peticiones. En el laboratorio, cada 5 segundos hace una petición HTTP al puerto 3000, ruta `/`. Si la VM falla varias veces seguidas (por defecto 2), el balanceador la **saca de rotación** y deja de mandarle tráfico hasta que vuelva a responder. Así los usuarios no reciben errores por una VM caída, y se evita enviar carga a una máquina que no funciona.

Un detalle útil para tu análisis: la función de Fibonacci es muy intensiva en CPU y Node.js ejecuta el código en un solo hilo. Bajo mucha carga, la VM puede tardar en contestar al sondeo y ser marcada como no saludable, aunque siga viva.

## 4. Load Balancing Rule y persistencia de sesión

**Propósito.** Define cómo se reparte el tráfico: asocia una IP y puerto de front-end con un pool y un puerto de back-end, junto con el protocolo, el sondeo de estado, el modo de persistencia y el tiempo de inactividad. En el laboratorio, la regla recibe tráfico TCP en el puerto 80 de la IP del balanceador y lo envía al puerto 3000 de las VMs sanas.

**Tipos de persistencia de sesión** (modo de distribución):

- **Ninguna** (hash de 5 elementos: IP y puerto de origen, IP y puerto de destino, protocolo): cada conexión puede ir a una VM distinta. Es el modo por defecto y el que reparte mejor.
- **IP de cliente** (2 elementos: IP de origen y destino): todas las peticiones del mismo cliente van a la misma VM.
- **IP de cliente y protocolo** (3 elementos): igual, pero separando por protocolo.

**Por qué importa y cómo afecta la escalabilidad.** Si la aplicación guarda estado en la memoria de cada servidor (sesión de usuario, carrito de compras), el cliente debe volver siempre a la misma VM, y por eso se usa persistencia. Pero tiene un costo:

- El reparto deja de ser parejo: muchos usuarios detrás de una misma IP (una universidad, una oficina con NAT) caen todos en una sola VM y la saturan mientras las demás descansan.
- Si esa VM cae, los usuarios pierden su sesión.
- Dificulta escalar, porque agregar VMs no alivia la que ya tiene los clientes asignados.

La buena práctica es diseñar aplicaciones **sin estado** (guardando la sesión en una base de datos o caché externa) y usar persistencia **Ninguna**. La app de Fibonacci no guarda estado, por eso encaja con ese modo.

## 5. Virtual Network, Subnet y rangos de direcciones

- **Virtual Network (VNet):** es tu red privada y aislada dentro de Azure. Permite que VMs, balanceadores y otros recursos se comuniquen entre sí de forma segura, y controla qué se conecta con internet o con redes externas.
- **Subnet:** es una subdivisión de la VNet. Sirve para organizar los recursos por función (web, base de datos, administración) y aplicar reglas distintas a cada grupo, como un NSG o una tabla de rutas propios.
- **Address space (espacio de direcciones):** es el bloque completo de IPs privadas de la VNet, escrito en notación CIDR. En el laboratorio, `10.0.0.0/16`, con 65.536 direcciones.
- **Address range (rango de direcciones de la subred):** es la porción del espacio de la VNet asignada a una subred. Aquí, `10.0.0.0/24`, con 256 direcciones (Azure reserva 5, así que quedan 251 utilizables).

El rango de cada subred debe quedar dentro del espacio de la VNet y no solaparse con otras subredes. Conviene que el espacio no choque con otras VNets o redes locales a las que luego quieras conectarte.

## 6. Zonas de disponibilidad e IP zone-redundant

**Zonas de disponibilidad:** son centros de datos físicamente separados dentro de una misma región, cada uno con su propia energía, refrigeración y red. Si uno falla por un corte eléctrico, un incendio o un problema de red, los otros siguen funcionando.

**Por qué 3 zonas distintas.** Poniendo VM1, VM2 y VM3 en las zonas 1, 2 y 3, la caída de un centro de datos completo deja funcionando las otras dos VMs, y el balanceador sigue atendiendo. Con las tres VMs en la misma zona, un solo fallo físico dejaría todo el sistema caído. Además, Azure ofrece un SLA del 99,99 % para VMs distribuidas en 2 o más zonas.

**IP zone-redundant.** Significa que una única dirección IP pública se sirve simultáneamente desde todas las zonas de la región. Si una zona falla, la IP sigue disponible desde las demás, y el cliente no nota el cambio. Es distinta de una IP **zonal**, que está atada a una sola zona y cae con ella. Si el front-end del balanceador no fuera redundante en zona, sería el punto único de falla aunque las VMs estén repartidas.

## 7. Network Security Group (NSG)

Es un firewall virtual que filtra el tráfico de red hacia y desde los recursos de Azure. Contiene reglas con prioridad (el número menor se evalúa primero), cada una con origen, destino, puerto, protocolo y acción (permitir o denegar). Se asocia a una **subred** o a la **NIC** de una VM.

Con SKU Standard, todo el tráfico entrante desde internet está bloqueado por defecto, así que el NSG es el que decide qué entra. En el laboratorio, las dos reglas de entrada permiten el puerto **22** (SSH, para administrar las VMs) y el **3000** (la aplicación de Fibonacci). Las reglas por defecto (como `AllowAzureLoadBalancerInBound`, prioridad 65001) permiten además que el sondeo de estado del balanceador llegue a las VMs, y `DenyAllInBound` bloquea todo lo demás. Es una capa de seguridad básica que reduce la superficie de ataque.
