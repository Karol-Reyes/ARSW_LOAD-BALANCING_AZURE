EP-01.F1.US2 — Inicio de sesión de usuario

Tarea 1: Diseñar el modelo de usuarios precargados y el bloqueo temporal
Descripción: Definir la tabla/colección de usuarios precargados por rol y la regla de bloqueo tras 3 intentos fallidos durante 5 minutos.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar la autenticación contra usuarios precargados
Descripción: Endpoint que valida correo/contraseña contra los usuarios precargados y aplica el bloqueo temporal.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 3: Implementar la pantalla de login y manejo de sesión
Descripción: Formulario de login en el frontend, manejo del token/sesión y redirección según el rol.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 4: Probar credenciales correctas, incorrectas y bloqueo
Descripción: Verificar los 2 escenarios Gherkin de la historia: login exitoso y bloqueo tras 3 intentos fallidos.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-02.F1.US1 — Consultar el mapa

Tarea 1: Diseñar la vista base del mapa
Descripción: Definir qué elementos (zonas, centros, ambulancias) se cargan al abrir el mapa y su representación visual.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 3

Tarea 2: Implementar el endpoint de elementos activos del mapa
Descripción: Endpoint que retorna zonas y centros activos con su ubicación geográfica.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 3: Implementar el renderizado del mapa en el frontend
Descripción: Cargar y pintar en el mapa los elementos activos devueltos por el backend.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 5

Tarea 4: Probar la carga inicial del mapa
Descripción: Verificar que, con zonas y centros registrados, el mapa muestra todos los elementos activos correctamente ubicados.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-02.F1.US2 — Recibir alertas del mapa en tiempo real

Tarea 1: Diseñar el canal de tiempo real del mapa (WebSocket)
Descripción: Definir el protocolo de conexión persistente y los eventos que se propagan (crear/editar elemento).
Prioridad: 1 (Crítica)
Actividad: Design
Horas estimadas: 3

Tarea 2: Implementar la emisión de eventos al crear/editar un elemento
Descripción: Backend que publica el evento a todos los clientes conectados cuando el mapa cambia.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 6

Tarea 3: Implementar la suscripción y actualización en vivo en el frontend
Descripción: El cliente recibe el evento y actualiza el mapa sin recargar la página.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 5

Tarea 4: Probar la propagación con dos sesiones simultáneas
Descripción: Verificar que un cambio hecho por el usuario A se refleja en pocos segundos en la pantalla del usuario B.
Prioridad: 1 (Crítica)
Actividad: Testing
Horas estimadas: 3

EP-02.F1.US3 — Reportar una zona afectada en el mapa

Tarea 1: Diseñar el formulario de reporte de zona
Descripción: Definir los campos mínimos: tipo de emergencia y ubicación.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de creación de zona afectada
Descripción: Endpoint que registra la zona y la deja visible de inmediato en el mapa.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar el formulario de reporte en el frontend
Descripción: Formulario para que el ciudadano reporte una zona desde el mapa.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el reporte exitoso de una zona
Descripción: Verificar que, al reportar, la zona queda visible de inmediato para los centros de ayuda cercanos.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-02.F2.US1 — Conocer la cantidad de recursos de un centro por medio de un semáforo

Tarea 1: Diseñar las reglas y umbrales del semáforo
Descripción: Definir los porcentajes de inventario que determinan rojo, amarillo y verde.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el cálculo del semáforo por centro de acopio
Descripción: Lógica que calcula el color del semáforo a partir del inventario disponible frente al mínimo necesario.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 3: Implementar la visualización del semáforo en el mapa
Descripción: Mostrar el color y el ícono del semáforo sobre cada centro de acopio en el mapa.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el recalculo automático tras un cambio de inventario
Descripción: Verificar los 3 escenarios: rojo por escasez, amarillo en el mínimo, y recalculo automático tras una reposición.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-03.F1.US1 — Solicitar un recurso a un centro

Tarea 1: Diseñar el flujo de solicitud de un recurso
Descripción: Definir los datos que viajan en la solicitud (recurso, cantidad, centro de acopio).
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de solicitud de un recurso
Descripción: Endpoint que registra la solicitud y la deja lista para el proceso de asignación.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar el formulario de solicitud en el frontend
Descripción: Formulario para que el administrador de centro de ayuda solicite un recurso disponible.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar la solicitud sobre un recurso con disponibilidad
Descripción: Verificar que una solicitud sobre un recurso con al menos 1 unidad disponible entra al proceso de asignación.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-03.F1.US2 — Confirmar la asignación de un recurso

Tarea 1: Diseñar la garantía transaccional de asignación atómica
Descripción: Definir el mecanismo (actualización condicionada en base de datos) que evita sobreasignación bajo concurrencia.
Prioridad: 1 (Crítica)
Actividad: Design
Horas estimadas: 4

Tarea 2: Implementar la asignación atómica del recurso
Descripción: Lógica de asignación con actualización condicionada (ej. UPDATE ... WHERE disponible > 0) para garantizar exclusión mutua real.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 8

Tarea 3: Probar la concurrencia con múltiples solicitantes simultáneos
Descripción: Verificar los 3 escenarios Gherkin: único solicitante, 5 solicitantes compitiendo por 1 unidad, y recurso sin stock.
Prioridad: 1 (Crítica)
Actividad: Testing
Horas estimadas: 5

Tarea 4: Documentar el mecanismo de concurrencia en la Wiki
Descripción: Registrar en la Wiki cómo se garantiza la asignación atómica, para la sustentación del reto de concurrencia.
Prioridad: 2 (Alta)
Actividad: Documentation
Horas estimadas: 2

EP-03.F1.US5 — Generar una alerta en el centro de acopio por escasez de un recurso

Tarea 1: Diseñar la condición y visualización de la alerta de escasez
Descripción: Definir cuándo se genera la alerta (solicitud > stock disponible) y su ícono en el mapa.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar la generación de la alerta por escasez
Descripción: Lógica que crea la alerta cuando la cantidad solicitada supera el stock disponible del centro.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 3: Implementar la visualización de la alerta en el mapa
Descripción: Mostrar el ícono de alerta sobre el centro de acopio afectado, con el recurso y la cantidad faltante.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el escenario de solicitud que supera el stock
Descripción: Verificar que, al solicitar más unidades de las disponibles, se muestra la alerta correcta.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-03.F2.US1 — Registrar el inventario inicial de un centro

Tarea 1: Diseñar el modelo de inventario por centro de acopio
Descripción: Definir la estructura de recurso, cantidad y centro asociado.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de registro de inventario inicial
Descripción: Endpoint que registra uno o varios recursos con su cantidad inicial para un centro.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar el formulario de registro de inventario
Descripción: Formulario para cargar el inventario inicial de un centro de acopio.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el registro inicial con varios recursos
Descripción: Verificar que, al registrar varios recursos, todos quedan disponibles para solicitud.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-03.F3.US1 — Registrar una donación

Tarea 1: Diseñar el modelo de donación y su estado pendiente
Descripción: Definir los campos de la donación (tipo, cantidad, estado) y sus transiciones posibles.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de registro de una donación
Descripción: Endpoint que crea la donación en estado pendiente.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar el formulario de registro de donación
Descripción: Formulario para que el donante indique tipo y cantidad a donar.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el registro de una donación pendiente
Descripción: Verificar que, al registrar, la donación queda visible como pendiente para los centros de acopio.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-03.F3.US4 — Consultar las alertas de donación pendientes

Tarea 1: Diseñar la vista de alertas pendientes por centro de acopio
Descripción: Definir cómo se listan las alertas activas y su información (recurso, cantidad faltante).
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint que lista las alertas activas
Descripción: Endpoint que retorna los centros de acopio con alertas de escasez vigentes.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar la vista de alertas para el donante
Descripción: Vista en el frontend donde el donante consulta las alertas pendientes.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar el listado de alertas con varios centros
Descripción: Verificar que, con 2 centros con alertas activas, se ven ambos con su recurso y cantidad faltante.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-04.F1.US1 — Ubicar las ambulancias en el mapa en tiempo real

Tarea 1: Diseñar el canal en tiempo real y la posición simulada
Descripción: Definir el protocolo de posición continua (cada 5s), el estado de reconexión y el de pérdida de señal.
Prioridad: 1 (Crítica)
Actividad: Design
Horas estimadas: 3

Tarea 2: Implementar la simulación y difusión de posición de ambulancias
Descripción: Backend que actualiza y transmite la posición simulada de cada ambulancia activa cada 5 segundos.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 7

Tarea 3: Implementar la actualización en vivo de posición en el mapa
Descripción: El frontend actualiza la posición de cada ambulancia sin recargar, incluyendo el estado "sin señal".
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 6

Tarea 4: Probar múltiples clientes, reconexión y pérdida de señal
Descripción: Verificar los 4 escenarios Gherkin: actualización a un usuario, propagación a 10 usuarios, conexión con ambulancias en movimiento, y pérdida de señal.
Prioridad: 1 (Crítica)
Actividad: Testing
Horas estimadas: 4

EP-04.F1.US4 — Diferenciar visualmente una ambulancia disponible de una ocupada

Tarea 1: Diseñar los estilos e íconos de disponible/ocupada
Descripción: Definir color e ícono distintivo para cada estado, evitando depender solo del color.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el estado de la ambulancia en el backend
Descripción: Exponer el estado disponible/ocupada de cada ambulancia en la información del mapa.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar la diferenciación visual en el mapa
Descripción: Pintar cada ambulancia con el color e ícono correspondiente a su estado actual.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar la visualización en ambos estados
Descripción: Verificar que una ambulancia disponible y una ocupada se muestran con estilos distintos.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US1 — Solicitar una ambulancia disponible

Tarea 1: Diseñar el flujo de solicitud de ambulancia
Descripción: Definir los datos mínimos de la solicitud (ubicación del ciudadano) y su disparo del broadcast.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de solicitud de ambulancia
Descripción: Endpoint que registra la solicitud y dispara la notificación a las ambulancias disponibles.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 3: Implementar el botón de solicitud en el frontend del ciudadano
Descripción: Acción para que el ciudadano solicite una ambulancia desde el mapa.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar la solicitud con ambulancias disponibles cerca
Descripción: Verificar que, con ambulancias disponibles en un radio cercano, la solicitud queda registrada y se dispara la notificación.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-04.F2.US2 — Notificar la solicitud a las ambulancias disponibles cercanas

Tarea 1: Diseñar el broadcast excluyendo ambulancias ocupadas
Descripción: Definir el criterio de selección (disponibles en el radio de búsqueda) y la exclusión explícita de ocupadas.
Prioridad: 1 (Crítica)
Actividad: Design
Horas estimadas: 3

Tarea 2: Implementar el envío simultáneo de la notificación
Descripción: Backend que notifica al mismo tiempo a todas las ambulancias disponibles cercanas.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 5

Tarea 3: Probar que las ambulancias ocupadas no reciben notificación
Descripción: Verificar los 2 escenarios: notificación simultánea a varias disponibles, y ninguna notificación a una ocupada.
Prioridad: 1 (Crítica)
Actividad: Testing
Horas estimadas: 2

EP-04.F2.US3 — Visualizar el detalle de una solicitud notificada

Tarea 1: Diseñar la vista de detalle de una solicitud notificada
Descripción: Definir la información mínima a mostrar: ubicación de origen y distancia estimada.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el endpoint de detalle de una solicitud
Descripción: Endpoint que retorna el detalle de una solicitud notificada a una ambulancia.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Implementar la vista de detalle en el frontend del conductor
Descripción: Pantalla donde el conductor abre y consulta el detalle de la solicitud recibida.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 3

Tarea 4: Probar la consulta del detalle desde las notificaciones
Descripción: Verificar que, al abrir la solicitud, se ve la ubicación de origen y la distancia estimada.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US4 — Aceptar una solicitud de ambulancia

Tarea 1: Diseñar la garantía transaccional de aceptación única
Descripción: Definir el mecanismo de actualización condicionada para que solo un conductor gane la solicitud.
Prioridad: 1 (Crítica)
Actividad: Design
Horas estimadas: 4

Tarea 2: Implementar la aceptación con actualización condicionada
Descripción: Lógica de aceptación atómica que asigna la solicitud a un único conductor y descarta al resto.
Prioridad: 1 (Crítica)
Actividad: Development
Horas estimadas: 8

Tarea 3: Probar la concurrencia entre varios conductores aceptando
Descripción: Verificar los 2 escenarios: aceptación única y ganadora, y aceptación simultánea con un solo ganador y mensaje de "ya asignada" para los demás.
Prioridad: 1 (Crítica)
Actividad: Testing
Horas estimadas: 5

Tarea 4: Documentar el mecanismo de concurrencia de ambulancias
Descripción: Registrar en la Wiki cómo se garantiza que solo un conductor gane la solicitud, para la sustentación.
Prioridad: 2 (Alta)
Actividad: Documentation
Horas estimadas: 2

EP-04.F2.US5 — Rechazar una solicitud de ambulancia

Tarea 1: Diseñar el flujo de rechazo de una solicitud
Descripción: Definir qué pasa con la solicitud y con la ambulancia que rechaza.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar el endpoint de rechazo de una solicitud
Descripción: Endpoint que marca el rechazo de un conductor sin afectar a las demás ambulancias notificadas.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Implementar la acción de rechazo en el frontend del conductor
Descripción: Botón para que el conductor rechace la solicitud notificada.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 4: Probar que la solicitud rechazada sigue disponible
Descripción: Verificar que, tras el rechazo, la solicitud sigue visible para las demás ambulancias notificadas.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US6 — Dejar expirar una solicitud sin responder

Tarea 1: Diseñar el mecanismo de expiración por tiempo límite
Descripción: Definir el tiempo límite de respuesta y qué ocurre al vencerse.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 2

Tarea 2: Implementar el temporizador y la expiración automática
Descripción: Lógica que retira la solicitud de la pantalla del conductor tras vencer el tiempo límite, sin afectar a los demás.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 4

Tarea 3: Probar que la solicitud expirada sigue disponible para otros
Descripción: Verificar que, al vencerse el tiempo, la solicitud deja de mostrarse a ese conductor y sigue disponible para el resto.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 2

EP-04.F2.US7 — Marcar la ambulancia como ocupada al aceptar una solicitud

Tarea 1: Diseñar la transición de estado disponible a ocupada
Descripción: Definir el disparador (aceptar una solicitud) y el efecto sobre notificaciones futuras.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar el cambio de estado automático al aceptar
Descripción: Actualizar el estado de la ambulancia a ocupada de inmediato al confirmarse la aceptación.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Probar el cambio de estado inmediatamente después de aceptar
Descripción: Verificar que, al aceptar, el estado de la ambulancia cambia a ocupada sin demora.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US8 — Declarar la llegada de la ambulancia al destino

Tarea 1: Diseñar el registro de llegada con fecha y hora
Descripción: Definir el evento de llegada y su registro con marca de tiempo.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar el endpoint de declaración de llegada
Descripción: Endpoint que registra la llegada de la ambulancia a la solicitud en curso.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Implementar el botón de llegada en el frontend del conductor
Descripción: Acción para que el conductor declare su llegada al destino.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 4: Probar el registro de la llegada con fecha y hora
Descripción: Verificar que, al declarar la llegada, el sistema registra correctamente fecha y hora.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US9 — Confirmar la llegada de la ambulancia al ciudadano

Tarea 1: Diseñar la notificación de llegada para el ciudadano
Descripción: Definir cómo se entera el ciudadano de que la ambulancia llegó (consulta o notificación).
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar la consulta/endpoint de estado de llegada
Descripción: Endpoint que expone si la ambulancia asignada ya llegó al destino.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Implementar la vista de confirmación de llegada del ciudadano
Descripción: Pantalla donde el ciudadano ve que su ambulancia ya llegó.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 4: Probar la confirmación tras la declaración del conductor
Descripción: Verificar que, al declarar el conductor su llegada, el ciudadano ve la confirmación al revisar su solicitud.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US10 — Liberar la ambulancia como disponible al finalizar el servicio

Tarea 1: Diseñar la transición de estado ocupada a disponible
Descripción: Definir el disparador (finalizar servicio) y su efecto sobre futuras notificaciones.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar el endpoint de finalización de servicio
Descripción: Endpoint que cambia el estado de la ambulancia de ocupada a disponible.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Probar que la ambulancia liberada recibe nuevas notificaciones
Descripción: Verificar que, tras finalizar el servicio, la ambulancia vuelve a estado disponible y puede recibir solicitudes.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1

EP-04.F2.US11 — Cerrar la solicitud de ambulancia atendida

Tarea 1: Diseñar el estado final "atendida" de una solicitud
Descripción: Definir la transición de la solicitud a su estado final y su efecto en el listado de activas.
Prioridad: 2 (Alta)
Actividad: Design
Horas estimadas: 1

Tarea 2: Implementar el cierre automático de la solicitud
Descripción: Lógica que marca la solicitud como atendida al finalizar el servicio de la ambulancia.
Prioridad: 2 (Alta)
Actividad: Development
Horas estimadas: 2

Tarea 3: Probar que la solicitud atendida deja de figurar como activa
Descripción: Verificar que, tras el cierre, la solicitud aparece como atendida y no entre las activas del ciudadano.
Prioridad: 2 (Alta)
Actividad: Testing
Horas estimadas: 1