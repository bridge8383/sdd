# Plantilla: refinar una especificación de agente

Usa esta plantilla para mejorar un agente ya generado sin cambiar silenciosamente su misión ni sus permisos.

---

Revisa y refina la especificación de agente proporcionada. Compórtate como revisor de diseño de agentes para desarrollo de software: busca ambigüedades, instrucciones contradictorias, responsabilidades fuera de alcance, criterios no verificables y transiciones que permitan avanzar sin aprobación.

## Datos de entrada

- Rol: [PO/PM | ANALISTA | DESARROLLADOR | TESTER]
- Contexto común vigente: [PEGAR O REFERENCIAR 00-contexto-y-protocolo.md]
- Especificación actual: [PEGAR DOCUMENTO]
- Observaciones, defectos o cambios solicitados: [LISTA]
- Plataforma destino y formato requerido: [PLATAFORMA / FORMATO / POR DEFINIR]

## Revisión obligatoria

Comprueba explícitamente:

1. Que la misión y los límites corresponden al rol.
2. Que las entradas, entregables y criterios de calidad son concretos y coherentes.
3. Que distingue entre producir una entrega y recibir la confirmación humana.
4. Que no inicia la siguiente etapa sin confirmación explícita del responsable principal.
5. Que no inventa confirmaciones, hechos, resultados de herramientas o decisiones.
6. Que define cómo tratar preguntas abiertas, bloqueos, rechazo y retrabajo.
7. Que mantiene trazabilidad con las etapas y artefactos del contexto común.
8. Si el rol es Analista de Software, que posee de forma exclusiva la etapa **Análisis y Diseño de Software**.

## Formato de salida

### Hallazgos
Lista breve con severidad (`CRÍTICO`, `IMPORTANTE`, `MENOR`), sección afectada y motivo.

### Especificación refinada
Entrega el documento completo, listo para reemplazar la versión anterior. Conserva decisiones válidas y aplica los cambios solicitados; no introduzcas cambios de alcance sin señalarlos.

### Cambios realizados
Resume los cambios y la razón de cada uno.

### Decisiones pendientes
Enumera solo los puntos que requieren decisión humana. Si no hay, indica `Ninguna`.

No marques el rol como aprobado: la versión refinada también requiere revisión y confirmación del responsable que el equipo haya designado.
