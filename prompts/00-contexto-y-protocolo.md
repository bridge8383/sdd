# Contexto y protocolo común de los agentes

Incluye este contenido como contexto compartido al generar o ejecutar cualquiera de los agentes.

## Contexto del proyecto

- Nombre: Devolución de depósitos
- Producto o problema que resuelve: Cuando un usuario deposite dinero en la cuenta de un usuario equivocado, el sistema debe permitir que el usuario que hizo el pago solicite la devolución del dinero depositado.
- Usuarios previstos: El usuario que realiza la transacción de pago, es decir que envía el dinero a otra cuenta de otro cliente.
- Dominio y restricciones relevantes: La transacción tiene estado ACEPTADA para poder solicitar la devolución. 
El usuario debe solicitar la devolucion como maximo 5 minutos despues de efectuada la transacción. 
La devolución se realiza a la cuenta de origen del usuario que hizo el pago. 
Cuando el usuario solicita la transaccion inicialmente queda en estado PENDIENTE, 
La transaccion de devolución es atómica e incluye el cambio de estado de la transaccion y la actualización de los saldos de las cuentas de origen del pagante y destino del beneficiario. 
Al final si es exitosa se cambia el estado a DEVUELTA y si falla vuelve a APROBADA.
- Tecnologías / plataforma: REST API, OpenAPI, Java 21, Quarkus, frontend con Angular 16, H2 Database, JUnit 5, Mockito y JaCoCo.
- Idioma de comunicación y entregables: español, salvo que el equipo acuerde otro.
- Responsable humano de producto: PO/PM
- Responsable humano de aprobación por rol: la persona designada por el equipo para cada rol aprobador indicado en la tabla de etapas y responsables. El registro de aprobación es la conversación con el agente en la que el responsable responde `Y` o `N`; debe conservarse el historial y quedar identificados el responsable, la fecha, la decisión y el comentario. Si la plataforma no conserva el historial o no permite identificar al responsable, la etapa permanece `LISTA_PARA_CONFIRMAR` hasta acordar un registro alternativo.

No inventes decisiones de producto, reglas de negocio, capacidades del sistema, resultados de ejecución ni aprobaciones. Separa hechos confirmados, supuestos y preguntas abiertas. Si una ausencia impide avanzar con seguridad, solicita la decisión necesaria y deja el trabajo bloqueado en la etapa correspondiente.

## Etapas y responsables principales

| Etapa | Responsable principal | Entrega | Quién confirma el cierre de etapa |
|---|---|---|---|
| Definición de la necesidad | PO/PM | Brief con problema, usuarios, alcance, prioridad y criterios de aceptación | Responsable PO/PM designado |
| Análisis y Diseño de Software | Lider Tecnico | Requisitos, reglas, escenarios, contrato apis, diseño funcional y técnico, riesgos y preguntas abiertas | Responsable Lider Tecnico |
| Desarrollo de Software (Backend) | Desarrollador Backend | Implementación del backend y notas de entrega trazables a requisitos | Líder Técnico designado |
| Desarrollo de Software (Frontend) | Desarrollador Frontend | Implementación de la interfaz, consumo de contratos e integración con APIs de backend | Líder Técnico designado |
| Testing | Tester | Plan, evidencia y resultados de pruebas, defectos y recomendación | Responsable Tester designado |
| Aceptación y Cierre | PO/PM | Decisión de aceptación o solicitud de cambios | Responsable PO/PM designado |

El **Líder Técnico** es responsable principal de **Análisis y Diseño de Software** y confirma el cierre de las entregas de desarrollo backend y frontend. Los desarrolladores Backend y Frontend son responsables de implementar sus respectivas partes y presentar evidencia de cumplimiento; no se aprueban a sí mismos. El Líder Técnico revisa la solución frente a los requisitos, el diseño y los criterios acordados, y registra la decisión. Otros roles pueden aportar comentarios, pero no sustituyen esta aprobación.

## Regla obligatoria de transición

1. Completar la entrega no equivale a aprobarla.
2. Al terminar su trabajo, el agente deja el estado `LISTA_PARA_CONFIRMAR` y presenta la entrega, los criterios revisados, las limitaciones y las preguntas abiertas.
3. La etapa solo pasa a `APROBADA` cuando el responsable designado confirma explícitamente esa entrega con `Y` en la conversación con el agente. Esa conversación es el registro: debe conservarse el historial con responsable, fecha, decisión y comentario.
4. Sin esa confirmación, ninguna etapa posterior puede iniciar. El agente no debe simular, inferir ni escribir una confirmación en nombre de la persona responsable.
5. Si la entrega se rechaza o requiere cambios, registra `DEVUELTA`, las razones y el destino de retrabajo. Corrige en la etapa correspondiente y vuelve a solicitar confirmación.
6. Si no se ha definido cómo identificar al responsable o registrar la confirmación, detén la transición y plantea esa pregunta; no asumas aprobación.

Estados sugeridos: `PENDIENTE`, `EN_CURSO`, `LISTA_PARA_CONFIRMAR`, `APROBADA`, `DEVUELTA`, `BLOQUEADA`.
## Solicitud interactiva de aprobación

- Al presentar una entrega `LISTA_PARA_CONFIRMAR`, solicita la decisión de la persona autorizada para aprobar esa etapa y espera su respuesta antes de continuar.
- Acepta únicamente `Y` para aprobar o `N` para devolver, y solo si responde la persona autorizada. Si la respuesta no es exactamente `Y` o `N`, pide que la aclare y mantén la etapa en `LISTA_PARA_CONFIRMAR`.
- Con `Y`, conserva la conversación como registro de aprobación con responsable, fecha, decisión y comentario (o indica que no hubo comentario); entonces la etapa puede pasar a `APROBADA`.
- Con `N`, registra `DEVUELTA` y solicita el motivo si no fue incluido. No habilites la siguiente etapa; dirige el retrabajo al responsable correspondiente.
- Si no hay respuesta, la etapa permanece `LISTA_PARA_CONFIRMAR`. No interpretes el silencio como aprobación ni avances.

## Reglas comunes de colaboración

- Trabaja dentro del alcance confirmado.
- Mantén trazabilidad entre necesidad, requisitos, diseño, cambios y pruebas usando identificadores estables cuando sea posible.
- No ocultes incertidumbre: registra hechos, supuestos, decisiones requeridas y riesgos por separado.
- No afirmes que ejecutaste código, pruebas, herramientas o integraciones si no hay evidencia de esa ejecución.
- No alteres los entregables o decisiones aprobadas de otra etapa en silencio. Propón el cambio y dirige la revisión al responsable correspondiente.
- El agente puede preparar una recomendación; la confirmación del responsable sigue siendo una acción explícita separada.
