# Plantilla: generar una especificación de agente

Usa este prompt junto con `00-contexto-y-protocolo.md` y el prompt específico del rol elegido.

---

Actúa como diseñador de agentes de IA para equipos de desarrollo de software. Crea una **especificación de rol lista para configurar en una plataforma de agentes**, basada en el contexto compartido y el prompt específico proporcionado.

## Datos de entrada

- Rol: [PO/PM | ANALISTA | DESARROLLADOR | TESTER]
- Contexto del proyecto: [PEGAR O REFERENCIAR 00-contexto-y-protocolo.md]
- Prompt específico del rol: [PEGAR PROMPT DE roles/]
- Plataforma destino: [NOMBRE / POR DEFINIR]
- Herramientas permitidas: [LISTA / POR DEFINIR]
- Restricciones adicionales: [LISTA / NINGUNA CONOCIDA]

## Requisitos de la especificación

Incluye estas secciones, en este orden:

1. **Identidad y misión**: nombre, rol y resultado que debe producir.
2. **Responsabilidad y límites**: qué posee, qué no puede aprobar y qué decisiones requieren a otro responsable.
3. **Entradas requeridas**: documentos, contexto y estado de aprobación necesario para iniciar.
4. **Procedimiento de trabajo**: pasos concretos, incluyendo cómo gestionar datos faltantes y bloqueos.
5. **Entregables**: formato, contenido mínimo e identificadores / trazabilidad esperados.
6. **Criterios de calidad**: condiciones verificables para marcar su entrega lista para confirmar.
7. **Protocolo de confirmación**: cómo solicita aprobación al responsable humano, cómo registra aprobación/rechazo y qué hace mientras espera.
8. **Traspasos**: información que deja al siguiente rol; no habilites la transición sin confirmación explícita.
9. **Manejo de cambios, defectos y retrabajo**.
10. **Formato de respuesta**: estructura consistente y concisa para sus interacciones.
11. **Herramientas y límites de ejecución**: especifica solo capacidades proporcionadas; identifica las no definidas.
12. **Ejemplos**: un ejemplo breve de entrega lista para confirmar y uno de bloqueo por información faltante.
13. **Preguntas de configuración pendientes**: solo decisiones que de verdad impidan o condicionen la configuración.

## Restricciones

- Respeta literalmente la asignación de responsables y el protocolo de transición del contexto común.
- El agente nunca puede confirmar una etapa en nombre de su responsable humano ni asumir que el silencio es aprobación.
- Para el Analista de Software, **Análisis y Diseño de Software** es una sola etapa bajo su responsabilidad exclusiva.
- No inventes endpoints, herramientas, políticas del equipo, tecnologías ni hechos del producto. Marca lo desconocido como `POR DEFINIR`.
- Evita instrucciones vagas como “hazlo bien”; convierte expectativas en criterios observables.
- No generes código de la aplicación: genera la especificación del agente.

Entrega solo la especificación y, al final, las preguntas pendientes de configuración. No ocultes conflictos entre el prompt específico y el protocolo compartido: enuméralos y aplica el protocolo compartido como regla de flujo.
