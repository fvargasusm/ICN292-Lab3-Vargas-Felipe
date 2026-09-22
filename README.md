
# ICN292 - Laboratorio 3: Triage de devoluciones AndesHogar SpA (n8n)

**Autor:** Felipe Vargas
**RUT** (sin DV): 21.333.135, semilla S = 135, U = $65.000, D = 28 días
**Fecha:** 22 de septiembre de 2026
**Curso:** ICN-292 Sistemas de Información para la Gestión, USM
**Repositorio:** https://github.com/fvargasusm/ICN292-Lab3-Vargas-Felipe

## Archivos

| Archivo | Contenido |
|---|---|
| `ICN292-Lab3-Vargas-Felipe.pdf` | Informe (resumen ejecutivo, partes A, B y C, capturas) |
| `ICN292-Lab3-Vargas-Felipe.docx` | Mismo informe en Word |
| `ICN292-Lab3-Vargas-Felipe-triage.json` | Workflow de triage (Webhook → Switch Rules → Edit Fields → UF → registro → notificación → respuesta) |
| `ICN292-Lab3-Vargas-Felipe-emisor.json` | Workflow que envía las 15 solicitudes por POST al webhook |
| `ICN292-Lab3-Vargas-Felipe-resumen.json` | Workflow programado (20:00 diario) que consolida el día con Summarize y envía un mensaje |
| `ICN292-Lab3-Vargas-Felipe-registro.xlsx` | Copia del registro producido por el flujo, con las 15 filas |
| `capturas/` | Capturas de las ejecuciones citadas en el informe |

## Cómo reproducir

1. En n8n, menú del workflow → **Import from File**, e importar los tres `.json`.
2. Crear una planilla de Google llamada `ICN292-Lab3-Registro`, con una hoja llamada `Registro` y estos encabezados en la fila 1: `id_solicitud`, `sku`, `monto`, `dias_desde_compra`, `ruta`, `motivo`, `U_aplicado`, `D_aplicado`. Luego seleccionarla en los nodos **Guardar registro en planilla** (triage) y **Leer registro** (resumen), ya que el `.json` guarda el ID de la planilla de la instancia original.
3. Conectar una credencial de Google Sheets OAuth2 en esos dos nodos.
4. Publicar el workflow de triage y reemplazar en el nodo **POST al webhook de triage** del emisor la URL de producción por la de la nueva instancia.
5. Ejecutar el emisor con **Execute workflow**. El nodo **Cargas a enviar** tiene una constante `MODO` en la línea 8 que selecciona el lote: `produccion` (las 15 solicitudes), `frontera` (monto igual a U y días iguales a D), `invalidas` (id vacío, monto cero, monto negativo) e `imposibles` (monto en palabras y SKU inexistente).
6. Para probar el resumen sin esperar a las 20:00, ejecutarlo a mano con **Execute workflow**.

Los `.json` no contienen `pinData` ni credenciales (solo el nombre e ID de la credencial de Google Sheets).
