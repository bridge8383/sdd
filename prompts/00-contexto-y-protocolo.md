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
- Tecnologías / plataforma: REST API, OpenAPI, Java 21, Quarkus, frontend con angular 16, react, h2 database, JUnit5, Mockito, JaCoCo.
- Idioma de comunicación y entregables: español, salvo que el equipo acuerde otro.
- Responsable humano de producto: PO/PM
- Responsable humano de aprobación por rol: [PERSONA O REGLA DEL EQUIPO]

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
3. La etapa solo pasa a `APROBADA` cuando el responsable designado confirma explícitamente esa entrega. Registra responsable, fecha, decisión y comentario en el mecanismo de seguimiento acordado.
4. Sin esa confirmación, ninguna etapa posterior puede iniciar. El agente no debe simular, inferir ni escribir una confirmación en nombre de la persona responsable.
5. Si la entrega se rechaza o requiere cambios, registra `DEVUELTA`, las razones y el destino de retrabajo. Corrige en la etapa correspondiente y vuelve a solicitar confirmación.
6. Si no se ha definido cómo identificar al responsable o registrar la confirmación, detén la transición y plantea esa pregunta; no asumas aprobación.

Estados sugeridos: `PENDIENTE`, `EN_CURSO`, `LISTA_PARA_CONFIRMAR`, `APROBADA`, `DEVUELTA`, `BLOQUEADA`.

## Reglas comunes de colaboración

- Trabaja dentro del alcance confirmado. Expón cualquier cambio de alcance para decisión del PO/PM.
- Mantén trazabilidad entre necesidad, requisitos, diseño, cambios y pruebas usando identificadores estables cuando sea posible.
- No ocultes incertidumbre: registra hechos, supuestos, decisiones requeridas y riesgos por separado.
- No afirmes que ejecutaste código, pruebas, herramientas o integraciones si no hay evidencia de esa ejecución.
- No alteres los entregables o decisiones aprobadas de otra etapa en silencio. Propón el cambio y dirige la revisión al responsable correspondiente.
- El agente puede preparar una recomendación; la confirmación del responsable sigue siendo una acción explícita separada.
