# CLAUDE.md — Despachos y Fecha PCP (despachos-pcp)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
`index.html` muestra una fila por OP del reporte "Situación de Pedido Global". Junto a la FEC. DESPACHO del sistema (que aquí nunca se edita), PCP escribe la **FECHA PCP** acordada con el cliente.
- Orden por defecto: FEC. DESPACHO ascendente (orden de negocio, fuera de la regla 500 para el orden; sin filtros se dibujan 500 filas y con filtro todas).
- Columnas: "clave" (OP, cliente, P.O., estilo cliente, categoría, pedido, exportadas, % exportadas, FEC. DESPACHO) o todas, más Fecha PCP, Dif. (días) = FECHA PCP − FEC. DESPACHO, "Última carga" (vino / no vino) e "Historial (n)". Fila de total con % exportadas ponderado (Σ exportadas ÷ Σ pedido).
- Filtros: búsqueda (OP, P.O., estilo, grupo), cliente, mes de despacho, con o sin fecha PCP, y si vino en la última carga. KPIs: OP con fecha PCP, fecha distinta a la del sistema (posteriores y anteriores) y las que no vinieron en la última carga. Pestañas "OP y fecha PCP" y "Cargas realizadas". No hay meta.
- **Editar FECHA PCP**: se guarda sola al cambiar; vacía la borra. Si la misma P.O. del mismo cliente tiene otras OP, ofrece aplicar la fecha a todas (pide confirmar si alguna ya tenía otra).
- **Subir archivo**: el reporte Situación de Pedido Global (.xlsx o .xls); busca en las primeras 40 filas la fila con "OP" y "FEC. DESPACHO"; requiere OP, CLIENTE, P.O., FEC. DESPACHO, PEDIDO y EXPORTADAS; rechaza OP repetidas. Antes de guardar muestra: nuevas, se actualizan, sin cambios y no vinieron (se conservan, no se borran), cambios de FEC. DESPACHO y columnas nuevas o faltantes. La FECHA PCP nunca la toca una carga.
- **Historial por OP**: cambios de FECHA PCP (manual o por P.O.) y de FEC. DESPACHO (por carga).
- Excel LIVES con la lista filtrada y ordenada.

## Base de datos (Supabase lives-datos)
- `dp_leer` (base, fechas PCP, conteo de historial y cargas), `dp_cargar` (carga; el usuario lo toma la base de la sesión), `dp_fecha_pcp` (guardar fecha de una o varias OP; es idempotente), `dp_historial_op`.
- Tablas: `dp_base`, `dp_fechas_pcp`, `dp_cargas`, `dp_columnas`, `dp_historial`; quién edita en `dp_editores`.
- No usa Storage.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 1 persona (Analista de Planeamiento).

## Quién puede editar
- Lo decide la base: `dp_puede_editar_` (superusuario o lista `dp_editores`). `dp_leer` devuelve `puede_editar`; sin permiso la pantalla queda en "Solo consulta".
- A Sistemas: `dp_puede_editar_` es la pieza que reemplazan con su seguridad; el reporte debería salir directo de sus sistemas.

## Pendientes vistos al escribir esto (21/10/2026)
- El texto de ayuda de la pantalla nombra a la persona que edita: debe pasar a texto genérico.
- Constantes sin uso: `LIMITE_FILAS` y `V.limite`.
