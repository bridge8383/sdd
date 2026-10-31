# HU-FUNC-01 — Solicitar devolución de dinero de una transacción

## Estado

`BLOQUEADA` — borrador de Definición de la necesidad. Falta resolver preguntas que afectan el alcance y los criterios de aceptación. No está aprobada.

## Necesidad recibida

Se solicita especificar el cambio **HU-FUNC-01**, cuyo propósito es permitir la devolución de dinero de una transacción monetaria del sistema.

El contexto existente del producto, **Devolución de depósitos**, indica que cuando exista un error en una transacción de depósito, el sistema debe permitir al usuario solicitar la devolución del dinero depositado. No está confirmado si HU-FUNC-01 se limita a ese caso o cubre otros tipos de transacciones.

## Problema

`APROBADA`: El cliente no cuenta actualmente con un mecanismo para solicitar la devolución de dinero de una transacción monetaria del sistema, lo que puede generar insatisfacción y problemas operativos.

## Usuarios

`POR DEFINIR`: la solicitud es iniciada por un usuario del sistema.

## Resultado esperado

Como mínimo, se solicita que el sistema permita iniciar una devolución asociada a una transacción monetaria. Falta confirmar qué significa “devolución” en el producto: solicitud de devolución, aprobación, ejecución del movimiento de dinero o el proceso completo.

## Alcance propuesto para discusión

- HU-FUNC-01 trata sobre iniciar o gestionar la devolución de dinero relacionada con una transacción del sistema.
- La relación con el contexto previo de errores en depósitos debe confirmarse: podría limitarse a depósitos erróneos o incluir otras operaciones monetarias.
- No se incluyen todavía reglas de elegibilidad, autorización, aprobación, procesamiento, notificaciones ni conciliación; están pendientes de definición.

## Exclusiones

`APROBADA`: no se pueden devolver transacciones que tengan mas de un mes de efectuadas al dia de hoy o que tengan estado DEVUELTA.

## Criterios de aceptación

- La transaccion monetaria existe y está en estado 'COMPLETA'.
- La transacción queda en estado 'DEVUELTA' y se guarda la fecha y hora de la devolución en un campo nuevo.
- Las cuentas de origen y destino de la transacción se actualizan correctamente con el importe devuelto.

## Prioridad

`ALTA`.

## Restricciones, dependencias y riesgos

- Reglas del dominio monetario, legales, operativas y de seguridad: `NO`.
- Dependencia de sistemas de pagos, cuentas o servicios externos: `NO`.
- Hay ambigüedad entre el contexto existente —error en transacción de depósito— y la solicitud actual —transacción monetaria en general—. Resolverla cambia el alcance.
- No se debe interpretar “solicitar una devolución” como autorización para ejecutar automáticamente un reembolso.

## Hechos confirmados

- Identificador solicitado: `HU-FUNC-01`.
- El cambio se relaciona con devolver dinero de una transacción monetaria del sistema.
- El contexto del producto menciona que un usuario debe poder solicitar la devolución del dinero depositado cuando exista un error en la transacción de depósito.

## Supuestos

- Ninguno adoptado como regla del producto. Las posibles interpretaciones se mantienen como preguntas abiertas.

## Preguntas abiertas para PO/PM

1. ¿HU-FUNC-01 aplica únicamente a depósitos con error, como indica el contexto actual, o a cualquier tipo de transacción monetaria? ¿Qué tipos concretos?
2. ¿Quién puede iniciar la solicitud: el usuario dueño de la transacción, un operador interno u otro actor? ¿Se requieren roles o permisos distintos?
3. ¿Qué condiciones hacen elegible una transacción para devolución? ¿Cómo se define “error” y quién lo determina?
4. ¿El alcance funcional termina al registrar una solicitud, incluye su aprobación/rechazo o debe ejecutar y completar el movimiento de devolución?
5. ¿La devolución siempre es por el importe total o puede ser parcial? ¿Qué moneda y reglas de conversión, si aplican?
6. ¿Qué estados debe tener la solicitud/transacción y quién puede cambiar cada estado?
7. ¿Se requiere revisión/aprobación humana antes de procesar la devolución? ¿Por quién y bajo qué condiciones?
8. ¿Qué debe ocurrir ante solicitudes duplicadas, transacciones ya devueltas, cancelaciones o fallos al procesar el dinero?
9. ¿Qué información o evidencia debe aportar el usuario? ¿Se requiere motivo, comprobante o comentario?
10. ¿Qué confirmaciones o notificaciones debe recibir el usuario y por qué canal?
11. ¿Hay plazos, límites de monto, políticas financieras, requisitos legales o de auditoría que deban reflejarse?
12. ¿Cuál es la prioridad y qué resultado medible define el éxito?
13. ¿Quién es el responsable humano PO/PM que revisará y confirmará este brief?

## Confirmación requerida

Responsable humano PO/PM: `APROBADO`. Se requiere resolver las preguntas que determinen alcance y reglas, actualizar el brief y recibir confirmación explícita. Hasta entonces, Análisis y Diseño de Software permanece bloqueado.
