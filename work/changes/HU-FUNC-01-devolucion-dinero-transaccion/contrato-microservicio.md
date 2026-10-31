# Contrato de cambio del microservicio — HU-FUNC-01

## Estado

**Borrador de contrato funcional y API — `LISTA_PARA_CONFIRMAR`.**  
La operación de devolución se considera aprobada para este análisis por instrucción del solicitante. La propuesta OpenAPI está en `../../../src/main/resources/openapi.yaml`; este documento no confirma por sí solo la aprobación formal de la etapa Análisis y Diseño de Software.

## 1. Referencias

- Historia de usuario: `HU-FUNC-01`.
- Brief: `brief.md`.
- Registro de etapas y aprobaciones: `approvals.md`.
- Agente responsable del análisis: Analista de Software.

El brief mantiene la etiqueta `BLOQUEADA` y el registro de aprobaciones muestra Definición de la necesidad y Análisis y Diseño de Software como `APROBADA`, sin fechas ni comentarios completos. Se conserva esta discrepancia para que el responsable actualice el registro; el presente contrato no modifica `approvals.md`.

## 2. Propósito y alcance

Permitir devolver una transacción monetaria que cumpla las condiciones de elegibilidad, marcarla como devuelta, registrar cuándo ocurrió la devolución y actualizar los saldos de las cuentas involucradas.

La funcionalidad descrita en los criterios actuales es la ejecución de la devolución, no solo el registro de una solicitud. La mención anterior a depósitos con error y el alcance actual de “transacción monetaria” no coinciden. Para este borrador se usa el alcance del criterio más reciente: **transacción monetaria**; los tipos concretos deben confirmarse con PO/PM.

## 3. Reglas de negocio contractuales

Una transacción es elegible cuando:

1. Existe.
2. Su estado es `COMPLETA`.
3. No han transcurrido más de un mes desde su fecha de realización.
4. Su estado no es `DEVUELTA`.

Si cumple todas las condiciones, la devolución debe:

1. Reintegrar el importe de la transacción a las cuentas involucradas.
2. Cambiar el estado de la transacción a `DEVUELTA`.
3. Registrar la fecha y hora de devolución en un campo nuevo.

La operación debe dejar un resultado coherente: no puede reflejar la transacción como devuelta si los saldos no se actualizaron, ni modificar saldos y dejar la transacción como `COMPLETA`. La implementación debe garantizar consistencia y evitar una segunda devolución; el mecanismo técnico concreto queda pendiente del diseño del microservicio.

## 4. Contrato lógico de operación

**Nombre lógico:** `refundTransaction`, operación `POST /transactions/{transactionId}/refund` en `../../../src/main/resources/openapi.yaml`. Método y path son propuestas, no decisiones confirmadas por el brief.

### Entrada funcional mínima

| Campo | Tipo conceptual | Requerido | Definición |
|---|---|---:|---|
| `transactionId` | Identificador | Sí | Identifica la transacción que se solicita devolver. El formato está `POR DEFINIR`. |

No se incluyen actor, motivo o monto en la entrada porque el brief no los establece. La autorización, el motivo y la posibilidad de devolución parcial requieren decisión antes de cerrar el contrato de transporte.

### Resultado exitoso conceptual

| Campo | Tipo conceptual | Definición |
|---|---|---|
| `transactionId` | Identificador | Transacción procesada. |
| `status` | Estado | `DEVUELTA`. |
| `refundedAmount` | Importe monetario | Importe que se devolvió. La política de devolución parcial está `POR DEFINIR`. |
| `refundedAt` | Fecha y hora | Instante en que el sistema completó la devolución. Formato y zona horaria están `POR DEFINIR`. |

Estos nombres describen el resultado lógico. No fijan JSON, códigos HTTP, tópicos de mensajería ni nombres de campos persistentes definitivos.

## 5. Actualización de saldos

Interpretación propuesta para una reversión completa de la transacción original:

- Acreditar a la cuenta de origen el importe devuelto.
- Debitar de la cuenta de destino el importe devuelto.

Esta dirección es una **interpretación funcional pendiente de confirmación**. El criterio del brief solo dice que ambas cuentas se actualizan correctamente; no especifica el signo ni el movimiento contable de cada cuenta. La moneda, escala decimal, saldos insuficientes en destino y restricciones de cuenta también están `POR DEFINIR`.

## 6. Rechazos funcionales

La operación debe rechazarse sin aplicar la devolución cuando:

| Condición | Resultado lógico |
|---|---|
| No existe `transactionId` | Transacción no encontrada. |
| El estado no es `COMPLETA` | Transacción no elegible por estado. |
| La transacción tiene más de un mes | Transacción fuera del plazo de devolución. |
| El estado ya es `DEVUELTA` | Devolución ya realizada; no duplicar movimientos. |

`../../../src/main/resources/openapi.yaml` propone `400` para identificador mal formado, `404` para transacción inexistente, `409` para estado inválido/duplicado/conflicto concurrente, `422` para fuera de plazo y `500` para fallo inesperado. Estos códigos y su catálogo son propuestas y requieren confirmación.

## 7. Requisitos trazables

| ID | Requisito | Criterio fuente |
|---|---|---|
| `HU-FUNC-01-RF-01` | La operación debe localizar una transacción existente. | “La transacción monetaria existe”. |
| `HU-FUNC-01-RF-02` | Solo se devuelve una transacción en estado `COMPLETA`. | Criterio de aceptación. |
| `HU-FUNC-01-RF-03` | Se rechazan transacciones con más de un mes de antigüedad. | Exclusión aprobada en el brief. |
| `HU-FUNC-01-RF-04` | Se rechazan transacciones que ya están `DEVUELTA`. | Exclusión aprobada en el brief. |
| `HU-FUNC-01-RF-05` | Una devolución exitosa cambia el estado a `DEVUELTA`. | Criterio de aceptación. |
| `HU-FUNC-01-RF-06` | Una devolución exitosa registra su fecha y hora en el nuevo campo. | Criterio de aceptación. |
| `HU-FUNC-01-RF-07` | Una devolución exitosa actualiza las cuentas de origen y destino por el importe devuelto. | Criterio de aceptación. |
| `HU-FUNC-01-RNF-01` | Estado, fecha de devolución y saldos deben quedar consistentes ante éxito o fallo. | Necesidad técnica derivada de actualizar conjuntamente el estado y los saldos; confirmar mecanismo en diseño. |
| `HU-FUNC-01-RNF-02` | La operación no debe producir una segunda devolución ante solicitudes repetidas o concurrentes. | Derivado de la exclusión de transacciones ya `DEVUELTA`; política de respuesta/idempotencia por definir. |

## 8. Flujo lógico

1. Recibir la solicitud con el identificador de transacción.
2. Localizar la transacción; si no existe, rechazar.
3. Verificar estado `COMPLETA`; si no coincide, rechazar.
4. Verificar la antigüedad contra el límite de un mes; si lo excede, rechazar.
5. Verificar que no esté ya `DEVUELTA`; si lo está, rechazar sin repetir el movimiento monetario.
6. Aplicar la devolución a las cuentas con el importe correspondiente.
7. Actualizar la transacción a `DEVUELTA` y registrar la fecha/hora de devolución.
8. Confirmar el resultado exitoso únicamente cuando los cambios relacionados se hayan completado de forma consistente.

El orden físico de escrituras depende del modelo de persistencia. El servicio debe resolver atomicidad, concurrencia y recuperación sin exponer un éxito parcial.

## 9. Escenarios de prueba derivados

- Transacción existente, `COMPLETA` y dentro del límite: devolución exitosa; estado, fecha/hora y cuentas reflejan la operación.
- Identificador desconocido: rechazo y sin cambios en saldos ni transacción.
- Estado distinto de `COMPLETA`: rechazo y sin movimiento monetario.
- Transacción ya `DEVUELTA`: rechazo o respuesta idempotente según política confirmada; en ningún caso duplicar el movimiento.
- Transacción exactamente en el corte temporal y una inmediatamente anterior/posterior: esperado pendiente de definición precisa del mes.
- Error al actualizar una de las cuentas o la transacción: no dejar cambios parciales; comprobar recuperación según diseño.
- Dos solicitudes simultáneas para la misma transacción: solo una devolución efectiva.

Son casos sugeridos; no se han ejecutado pruebas.

## 10. Decisiones de diseño pendientes

Estas decisiones no impiden expresar las reglas funcionales anteriores, pero sí impiden publicar un contrato técnico definitivo o iniciar una implementación sin supuestos:

- Nombre del microservicio y límites de responsabilidad.
- Confirmar que el transporte será HTTP/REST y que el path, método, request/response y códigos de `../../../src/main/resources/openapi.yaml` son adecuados.
- Versionado de API y convención de errores/códigos.
- Formato/semántica del identificador de transacción.
- Definición exacta de “un mes”: mes calendario o duración fija, fecha de referencia, zona horaria y comportamiento en el día límite.
- Confirmar si el monto es siempre el total original o puede ser parcial.
- Confirmar la dirección contable propuesta para las cuentas origen y destino.
- Nombre y tipo del campo nuevo de fecha/hora, zona horaria y política de auditoría.
- Autenticación/autorización y actor permitido; el brief deja usuario/permisos sin definir.
- Idempotencia, concurrencia, estados ante fallos y estrategia de consistencia transaccional.
- Moneda, precisión decimal y condiciones de saldo insuficiente.
- Confirmar si aplican solo depósitos con error o cualquier transacción monetaria.

## 11. Estado de aprobación y traspaso

La operación funcional se analizó bajo la instrucción de considerarla aprobada. El contrato técnico queda `LISTA_PARA_CONFIRMAR` por el responsable humano de Análisis y Diseño de Software; los puntos anteriores deben cerrarse o aprobarse explícitamente como decisiones pendientes antes de Desarrollo.

El registro `approvals.md` debe conservar la aprobación humana real con responsable, fecha y comentario. Este documento no actualiza estados ni confirma etapas en nombre de nadie.
