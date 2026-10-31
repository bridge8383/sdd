# Prompts para diseñar agentes de desarrollo

Esta carpeta contiene prompts para **crear y refinar las especificaciones de los agentes** PO/PM, Analista de Software, Desarrollador de Software y Tester. No son todavía agentes ejecutables ni dependen de un proveedor de IA. El resultado de usarlos será una especificación de rol que luego podrás adaptar al formato de tu framework o plataforma.

## Archivos

- `00-contexto-y-protocolo.md`: contexto común, ciclo de vida y reglas de aprobación que todos los roles deben respetar.
- `01-generar-rol.md`: plantilla para generar la especificación inicial de un rol.
- `02-refinar-rol.md`: plantilla para revisar y mejorar una especificación existente.
- `roles/rol_po-pm.md`, `roles/rol_analista.md`, `roles/rol_desarrollador.md`, `roles/rol_tester.md`: prompts específicos para generar cada rol.

## Uso recomendado

1. Completa los datos del proyecto en `00-contexto-y-protocolo.md`.
2. Usa el prompt del rol correspondiente dentro de `roles/` para generar su primera especificación.
3. Guarda la respuesta como un archivo de especificación, por ejemplo `agents/po-pm.md` (esa carpeta aún no se crea: contendrá los agentes refinados).
4. Refina cada especificación con `02-refinar-rol.md`, pasando el documento actual y las observaciones recibidas.
5. Revisa manualmente el resultado y confirma que respeta el protocolo común antes de usarlo para configurar un agente.

Cada prompt pide una salida estructurada y distingue claramente entre **entrega de etapa** y **confirmación de aprobación**. La aprobación debe provenir del responsable humano designado; un agente no debe inventar una aprobación humana.

## Variables que debes completar

Reemplaza los campos entre corchetes —por ejemplo `[NOMBRE DEL PROYECTO]`— antes de ejecutar el prompt. Si un dato todavía no está decidido, déjalo como `POR DEFINIR`; el agente debe señalarlo como pregunta o supuesto, no inventar una política.
