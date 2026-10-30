# ARSW - Escalamiento en Azure: Parte 1 (Escalabilidad vertical)

## 1. ¿Cuántos y cuáles recursos crea Azure junto con la VM?

Al momento de realizar la creación de la VM en Azure, se generaron seis recursos:

1. Máquina virtual
2. Disco del sistema operativo
3. Interfaz de red (NIC)
4. Dirección IP pública
5. Grupo de seguridad de red (NSG)
6. Red virtual (con su subred)

## 2. ¿Para qué sirve cada recurso?

| Recurso | Función |
|---|---|
| Máquina virtual | Servidor donde corre Ubuntu y la aplicación (el CPU y la RAM). |
| Disco del SO | Es el almacenamiento persistente del sistema operativo y de los archivos. |
| NIC | Conecta la VM a la red virtual y le asigna una IP privada. |
| IP pública | Permite acceder a la VM desde internet. |
| NSG | Es el firewall que permite o bloquea tráfico según el puerto, protocolo y origen. |
| Red virtual / subred | Red privada donde viven la VM y otros recursos, con su rango de direcciones. |

## 3. ¿Por qué se cae la app al cerrar SSH y por qué hace falta la Inbound port rule?

**CAIDA DE LA APP**

Al ejecutar `node FibonacciApp.js`, el proceso queda atado a la sesión de terminal. Cuando se cierra el SSH, el sistema envía la señal (`SIGHUP`) y el proceso termina.
Aplicandole al comendo `forever` (o `nohup`, `systemd`) el proceso se desacopla de la sesión y sigue vivo.

**INBOUND PORT RULE**

El NSG (El firewall), por defecto, bloquea todo el tráfico entrante (lo marca como un `DenyAllInBound`) a excepción de lo que se permita explícitamente. Solo estaba abierto el puerto 22 y el puerto 80 originalmente. Después se le adicionó la del puerto-300, sin una regla para el puerto 3000, las peticiones nunca llegan a la aplicación.

## 4. Tabla de tiempos e interpretación

Inicialmente, se realizó la toma de tiempos cambiando el número correspondiente para cada prueba directamente en la URL, los resultados obtenidos en cada uno fueron los siguientes:

| n | Tiempo (ms) |
|---|---|
| 1000000 | 23900 |
| 1010000 | 24580 |
| 1020000 | 24980 |
| 1030000 | 25820 |
| 1040000 | 26920 |
| 1050000 | 27310 |
| 1060000 | 28280 |
| 1070000 | 27880 |
| 1080000 | 28250 |
| 1090000 | 28840 |

Más sin embargo, después de eso, con ayuda de la Inteligencia Artificial, se generó un script para pegar en consola que hacía todas las pruebas automáticamente y los resultados fueron los siguientes.

![tesst1](<test1.jpeg>)

**La interpretación obtenida fue la siguiente:** 

La función es un ciclo de n iteraciones que suma dos números gigantes con `big-integer`. El resultado tiene unos 209000 dígitos (lo cuál coincide con los 209 kB de la respuesta), así que cada suma recorre cientos de miles de dígitos y crea objetos nuevos. 

El costo crece más que linealmente con n, por eso los tiempos suben al aumentar n. Además, el cálculo es síncrono y bloquea el hilo de Node mientras dura.

## 5. Consumo de CPU e interpretación

![](<azure1.jpeg>)

![](<azure2.jpeg>)

![](<azure3.jpeg>)

![](<azure4.jpeg>)

En la gráfica de la VM de 2 vCPU el consumo de CPU llega a un pico de casi el 50% y nunca lo supera; hay otro pico intermedio de masomenos el 30%. Entre una prueba y otra la CPU vuelve a casi el 0%, y el promedio de las últimas 24h es por el 14%, porque incluye todo el tiempo en reposo.

La función solo hace aritmética, no necesita esperas de red ni de disco, así que mantiene un núcleo al 100 % mientras calcula. Node ejecuta JavaScript en un solo hilo, por lo que usa un solo núcleo. Con 2vCPU, un núcleo saturado equivale al 50 % del total, y es exactamente el tope que muestra la gráfica. El otro núcleo queda ocioso. (Los picos de casi 30% pueden deberse a que Azure promedia la CPU por intervalos de tiempo, lo que suaviza los picos reales.)

Esto muestra un problema de la métrica: el servicio está saturado y rechaza peticiones (ECONNRESET), pero la CPU nunca pasa del 70% que pedía el escenario de calidad. La CPU total de la VM no refleja que el único hilo de Node está al 100%.

Otras métricas de la captura (no relacionadas con la función). El pico de escritura en disco (1.3GiB, 2GiB en total) y de lectura (300MiB) ocurre justo antes de los picos de CPU. Corresponde a la instalación de Node, git clone y npm install, no al cálculo de Fibonacci, que no usa disco. La métrica de disponibilidad pasa de 0 a 1 cuando se creó la VM

**VM de 4vCPU (`VERTICAL-SCALABILITY-4VCPU`).** 

![](<azure5.jpeg>)

![](<azure6.jpeg>)

![](<azure7.jpeg>)

![](<azure8.jpeg>)

El pico llega a 24% (otros picos de 16% y 22%) y el promedio de las últimas 24 h es 12%. Es consistente con lo esperado: con 4vCPU, un núcleo saturado equivale al 25% del total. El otro 75% de la capacidad queda sin usar.
 
| VM | vCPU | Pico de CPU observado | Núcleos realmente saturados |
|---|---|---|---|
| `VERTICAL-SCALABILITY` (D2s_v3) | 2 | casi 50% | 1 |
| `VERTICAL-SCALABILITY-4VCPU` (E4s_v3) | 4 | casi 24% | 1 |

## 6. Resumen de ejecución de Postman (Newman)

Después de esas pruebas en navegador y las gráficas en Azure, se realizó la prueba en paralelo desde la terminal del computador y los resultados fueron los siguientes:

![prueba1](<prueba1.png>)

En Newman, `fibonacci/1000000` tardó unos 25 s como mínimo entre las 2 pruebas.
Dos corridas en paralelo sobre la VM de 2vCPU:

| | Fallos | Tiempo promedio | Mín / Máx | Duración total |
|---|---|---|---|---|
| Corrida 1 | 0 de 10 | 41 s | 25.1 s / 1m 3.3s | 6m 51s |
| Corrida 2 | 4 de 10 | 44 s | 24.7 s / 1m 2.6s | 5m 35s |

**Tiempos de cada petición.** Fueron entre 25s y más de 1min. Una petición sola tarda aproximadamente 25s; las demás esperan en cola mientras el hilo está ocupado, y por eso llegan a 1min.

**Fallos.** El error `read ECONNRESET`: la conexión se cortó antes de recibir la respuesta. Esto ocurre porque el hilo único está ocupado y las peticiones encoladas esperan demasiado.

## 7. Diferencia entre B2ms y B1ls

Ambas pertenecen a la serie B (*burstable*): no entregan CPU completa de forma sostenida, sino que acumulan créditos y los gastan cuando se necesita ráfaga. La diferencia real está en el rendimiento base:

- **B1ls:** 1vCPU, 0.5GB de RAM, rendimiento base muy bajo (alrededor del 5% de CPU). Sirve para tareas ligeras e intermitentes; con carga sostenida agota los créditos y se limita.
- **B2ms:** 2vCPU, 8GB de RAM, rendimiento base mucho mayor (alrededor del 60% del total) y más margen de ráfaga, aunque con carga sostenida al 100 % también acaba consumiendo sus créditos.

## Aclaración: tamaños utilizados en este laboratorio.

 El enunciado pide pasar de `B1ls` a `B2ms`, pero esos tamaños *no se utilizaron*. La suscripción Azure for Students, en la región East US2, no ofrecía tamaños de la serie B sin restricciones. Por eso se usaron estos:
 
| Rol en el laboratorio | Tamaño usado | vCPU | RAM | Serie |
|---|---|---|---|---|
| VM inicial (en lugar de `B1ls`) | `Standard_D2s_v3` | 2 | 8 GB | D v3, propósito general |
| VM escalada (en lugar de `B2ms`) | `Standard_E4s_v3` | 4 | 32 GB | E v3, optimizada para memoria |


La VM escalada no se pudo obtener de un redimensionado de la original (el portal no mostraba tamaños mayores disponibles). Por ello, se creó una **VM nueva**, `VERTICAL-SCALABILITY-4VCPU`, con la misma configuración y la app instalada de nuevo. Por eso las pruebas de 2 y 4 vCPU se hicieron sobre dos máquinas distintas, con IP públicas distintas.
 
Tampoco fue posible usar `D4s_v3` para escalar verticalmente (ya que está en una zona de disponibilidad no admitida) ni los `D4` de la serie v7 (porque aparecía cuota de la familia insuficiente), por lo que se eligió `E4s_v3` para duplicar los vCPU.
 
Estos tamaños no son *burstable*: la serie Dv3 y la Ev3 entregan CPU sostenida sin sistema de créditos. Por eso la diferencia de rendimiento base descrita arriba entre `B1ls` y `B2ms` no aplica directamente a nuestras pruebas. La comparación realizada fue **2vCPU contra 4vCPU**, y no 1 contra 2 como en el enunciado.

## 8. ¿Aumentar el tamaño de la VM es una buena solución? ¿Qué pasa con la app?

No. La aplicación es de un solo hilo, así que más vCPU casi no cambian su rendimiento. Al cambiar el tamaño la VM se reinicia, los procesos mueren y hay que volver a levantar la app, porque `forever` no arranca solo tras un reinicio salvo que se configure.

## 9. ¿Qué pasa con la infraestructura al cambiar el tamaño? Efectos negativos

Azure detiene la VM y la reasigna a otro hardware con el nuevo tamaño. Efectos negativos:

- **Downtime:** el servicio no responde durante el cambio.
- **Pérdida del estado en memoria** y tambien de los procesos en ejecución.
- **Disco temporal:** su contenido puede perderse.
- **Restricciones de disponibilidad:** no todos los tamaños están disponibles por región, zona o cuota (ocurrió con `B2ms` y `D4s_v3`).
- **Mayor costo por hora,** y siempre hay un tamaño máximo (no escala indefinidamente).
- **Punto único de fallo:** sigue habiendo una sola máquina.

## 10. ¿Hubo mejora en el consumo de CPU o en los tiempos de respuesta?

No. Los tiempos mínimos fueron casi iguales (casi 25s con 2vCPU y casi 27s con 4vCPU) y los fallos no bajaron (4 de 20 contra 5 de 20). El cálculo corre en un solo hilo: usa un solo núcleo sin importar cuántos haya, y los demás quedan ociosos.

**Sobre el consumo de CPU.** 

El porcentaje en Azure sí bajó (de casi 50% con 2vCPU a 24% con 4vCPU), pero no es una mejora real: en ambos casos hay un solo núcleo al 100%, y el porcentaje cambia solo porque se reparte entre más núcleos. El trabajo hecho y el tiempo de cada petición son los mismos.

## 11. Con 4 ejecuciones paralelas de Postman, ¿el comportamiento es porcentualmente mejor?

No, de hecho, el comportamiento es mucho peor.

| Prueba | Fallos | % fallos | Tiempo promedio |
|---|---|---|---|
| 2 vCPU, 2 corridas | 4 de 20 | 20 % | 41 s y 44 s |
| 4 vCPU, 2 corridas | 5 de 20 | 25 % | 48.6 s y 44 s |
| 4 vCPU, 4 corridas | 16 de 40 | **40 %** | 1m 10s a 1m 46s |

Con 4 corridas hubo más peticiones compitiendo por un único hilo, la cola creció, los tiempos subieron (máximo de 2m 44s) y más conexiones se cortaron con `read ECONNRESET`.

![](<prueba2.jpeg>)

## Conclusión sobre el escenario de calidad

El escenario exigía responder todas las peticiones con CPU por debajo del 70%.

Más sin embargo *No se cumple* 

Hubo entre 20% y 40% de peticiones fallidas. El escalamiento vertical no resuelve el problema porque la aplicación no aprovecha varios núcleos. Se necesita escalamiento horizontal (varias instancias detrás de un balanceador de carga), que es el objetivo que vamos a ver en la Parte 2.