# Especificación del MVP — CuentaClara

> Documento vivo. Si una decisión cambia, se actualiza acá primero y después en el código.

## 1. Resumen

**Propuesta de valor:** Que el tendero sepa exactamente quién le debe, cuánto y desde cuándo, y pueda cobrar sin discutir.

**Problema:** Los fiados se anotan en un cuaderno que se moja, se pierde o se discute; no se sabe cuánto hay por cobrar y cobrar a un vecino se vuelve incómodo.

**Cómo se resuelve hoy:** Cuaderno de fiados y memoria.

**Usuario objetivo:** Dueño de una tienda de barrio con 20 a 300 clientes que compran a crédito.

**Métricas de éxito del MVP:**

- Registrar un fiado en menos de 10 segundos.
- Ver el total por cobrar y los 5 mayores deudores en Inicio.
- Enviar un estado de cuenta en PDF por WhatsApp en menos de 15 segundos.

## 2. Alcance del MVP

### Incluido

- Clientes y saldos: deuda total y antigüedad por cliente.
- Fiados y abonos: registra ventas a crédito y pagos parciales.
- Estado de cuenta en PDF para compartir.
- Recordatorio de cobro por WhatsApp pre-llenado.
- Resumen: total por cobrar y mayores deudores.
- Respaldo local exportable.

### Fuera del MVP

- Cuentas de usuario, inicio de sesión y multiusuario.
- Servidor propio, nube o sincronización entre dispositivos.
- Pagos en línea o pasarelas de cobro.
- Publicación en App Store (iOS).
- Cualquier servicio de pago o con costo recurrente.

## 3. Reglas de negocio

Reglas que el código **debe** cumplir. Cada una se cubre con pruebas automáticas cuando es lógica pura.

- **RN-01** Un movimiento es un fiado (aumenta la deuda) o un abono (la reduce).
- **RN-02** Saldo del cliente = suma de fiados − suma de abonos, sin contar los movimientos anulados.
- **RN-03** Un abono mayor al saldo se rechaza o se registra como saldo a favor tras confirmación.
- **RN-04** Si el cliente tiene límite, un fiado que lo supere pide confirmación explícita.
- **RN-05** Los movimientos no se borran: se anulan con una marca y se excluyen del saldo.
- **RN-06** La antigüedad de la deuda cuenta desde el fiado pendiente más antiguo (método primero en entrar, primero en pagar).
- **RN-07** El estado de cuenta lista los movimientos no anulados y el saldo final a la fecha.

## 4. Historias de usuario y criterios de aceptación

Formato: **Dado** un contexto, **Cuando** el usuario hace algo, **Entonces** pasa algo verificable.

### HU-01 — Registrar clientes

**Como** tendero **quiero** cargar clientes con teléfono y límite opcional **para** tener mi libreta ordenada.

**Fase:** 2 — Clientes y movimientos

**Criterios de aceptación:**

1. **Dado:** no hay clientes. **Cuando:** abro la app. **Entonces:** veo el estado vacío con «Agregar cliente».
2. **Dado:** un cliente con teléfono repetido. **Cuando:** intento guardarlo. **Entonces:** la app avisa y ofrece abrir el existente.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-02 — Registrar fiados y abonos

**Como** tendero **quiero** anotar lo que se fía y lo que se paga **para** saber el saldo real.

**Fase:** 2 — Clientes y movimientos

**Criterios de aceptación:**

1. **Dado:** un cliente con saldo 0. **Cuando:** registro un fiado de 45 Bs. **Entonces:** su saldo es 45 Bs y aparece el movimiento con fecha.
2. **Dado:** un cliente con saldo 45. **Cuando:** registro un abono de 20. **Entonces:** su saldo baja a 25 Bs.
3. **Dado:** un fiado que supera el límite del cliente. **Cuando:** intento guardarlo. **Entonces:** la app pide confirmación.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-03 — Anular un movimiento

**Como** tendero **quiero** anular un movimiento registrado por error **para** corregir sin perder el rastro.

**Fase:** 2 — Clientes y movimientos

**Criterios de aceptación:**

1. **Dado:** un fiado equivocado. **Cuando:** lo anulo. **Entonces:** queda marcado como anulado, se excluye del saldo y sigue visible en el historial.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-04 — Ver saldos y antigüedad

**Como** tendero **quiero** ver cuánto debe cada cliente y desde cuándo **para** priorizar los cobros.

**Fase:** 2 — Clientes y movimientos

**Criterios de aceptación:**

1. **Dado:** clientes con deudas. **Cuando:** abro Clientes. **Entonces:** veo la lista ordenada por saldo con los días de antigüedad.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-05 — Estado de cuenta en PDF

**Como** tendero **quiero** generar el estado de cuenta de un cliente **para** mostrarle su detalle.

**Fase:** 3 — Estados de cuenta en PDF

**Criterios de aceptación:**

1. **Dado:** un cliente con movimientos. **Cuando:** toco «Estado de cuenta». **Entonces:** se genera un PDF con movimientos y saldo final y se abre el menú de compartir.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-06 — Cobrar por WhatsApp

**Como** tendero **quiero** enviar un recordatorio de cobro prellenado **para** cobrar sin discutir.

**Fase:** 4 — Recordatorios por WhatsApp

**Criterios de aceptación:**

1. **Dado:** un cliente con deuda y teléfono. **Cuando:** toco «Cobrar». **Entonces:** se abre WhatsApp con su nombre y el saldo.
2. **Dado:** un cliente sin teléfono. **Cuando:** veo su ficha. **Entonces:** el botón está deshabilitado y sugiere cargar el teléfono.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

### HU-07 — Resumen y respaldo

**Como** tendero **quiero** ver el total por cobrar y respaldar mis datos **para** no perder la libreta.

**Fase:** 5 — Respaldo y restauración

**Criterios de aceptación:**

1. **Dado:** clientes con deudas. **Cuando:** abro Inicio. **Entonces:** veo total por cobrar y los 5 mayores deudores.
2. **Dado:** quiero respaldar. **Cuando:** toco «Exportar respaldo». **Entonces:** se genera un archivo que puedo guardar o compartir y luego restaurar en otro teléfono.

Criterios comunes a toda historia:

- [ ] Funciona sin conexión a internet.
- [ ] Los datos persisten al cerrar y reabrir la app.
- [ ] Los errores de validación se muestran en español y junto al campo.
- [ ] Hay un estado vacío claro cuando todavía no hay datos.

## 5. Pantallas y navegación

| Pantalla | Qué muestra | Acciones principales |
|---|---|---|
| Inicio | Total por cobrar y mayores deudores | Registrar fiado o abono |
| Clientes | Lista con saldo y antigüedad, búsqueda | Crear, abrir ficha |
| Ficha de cliente | Saldo, historial de movimientos y datos | Fiado, abono, estado de cuenta, cobrar por WhatsApp |
| Formulario de movimiento | Tipo, monto, nota, fecha | Guardar |
| Ajustes | Nombre de la tienda, plantilla de cobro | Guardar |
| Respaldo | Exportar y restaurar el archivo de datos | Exportar, restaurar |

**Navegación:** la pantalla de inicio es el punto de partida; desde ahí se llega a cada sección con un toque y se vuelve con el botón atrás. Las acciones frecuentes (nuevo registro) están siempre a la vista. La navegación se implementa con Expo Router (rutas por archivo en `app/`).

## 6. Modelo de datos

Base SQLite local versionada con `PRAGMA user_version`. Las fechas se guardan como texto ISO 8601 y los montos como números reales (en bolivianos).

### `clientes`

Clientes con teléfono y límite de crédito opcional.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `nombre` | TEXT | Obligatorio |
| `telefono` | TEXT | Opcional |
| `limite` | REAL | Opcional |
| `activo` | INTEGER | Obligatorio; Por defecto 1 |

### `movimientos`

Cada fiado o abono con su monto, nota y fecha; los errores se anulan sin borrarse.

| Campo | Tipo | Reglas |
|---|---|---|
| `id` | INTEGER | Clave primaria autoincremental |
| `cliente_id` | INTEGER | Obligatorio; Referencia a clientes |
| `tipo` | TEXT | Obligatorio |
| `monto` | REAL | Obligatorio |
| `nota` | TEXT | Opcional |
| `fecha` | TEXT | Obligatorio |
| `anulado` | INTEGER | Obligatorio; Por defecto 0 |

### `ajustes`

Configuración clave-valor (nombre de la tienda, plantilla de mensaje).

| Campo | Tipo | Reglas |
|---|---|---|
| `clave` | TEXT | Clave primaria |
| `valor` | TEXT | Obligatorio |

Detalle de la política de migraciones: nunca editar una migración publicada; agregar una nueva al final de la lista en `src/db/schema.ts`.

## 7. Casos borde a contemplar

- Cliente que paga de más: saldo a favor con confirmación.
- Cliente con el mismo nombre: distinguir por teléfono y nota.
- Fecha de un fiado antigua: permitida para cargar el cuaderno.
- Archivo de respaldo de una versión más nueva del esquema: rechazar con mensaje claro.
- Cliente dado de baja con saldo: aparece en deudores hasta saldarse.

## 8. Requisitos no funcionales

- **Offline total:** ninguna función del MVP requiere internet.
- **Rendimiento:** la app abre en menos de 2 segundos; las listas responden de inmediato con cientos de registros (consultas con índices y listas virtualizadas).
- **Privacidad:** los datos nunca salen del teléfono, salvo lo que el usuario comparte a propósito.
- **Idioma y formato:** español de Bolivia, montos en bolivianos (Bs) con dos decimales, fechas día/mes/año.
- **Usabilidad:** botones grandes, formularios cortos y flujos de pocos toques; pensado para usarse de pie y con una mano.
- **Accesibilidad básica:** contraste suficiente, etiquetas de accesibilidad en botones y tamaño de texto que respeta el ajuste del sistema.
- **Calidad:** `npm run lint`, `npx tsc --noEmit` y `npm test` pasan en cada commit (CI en GitHub Actions).
- **Compatibilidad:** Android 8 o superior (probado con Expo Go y con el APK).

### Capacidades de Expo que usa

| Paquete | Para qué | Permiso |
|---|---|---|
| `expo-print` | Generar PDF desde HTML | — |
| `expo-sharing` | Compartir PDF, CSV o respaldos con otras apps | — |
| `expo-file-system` | Guardar y leer archivos locales (CSV, respaldos) | — |

## 9. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Pérdida del teléfono | Respaldo exportable con recordatorio semanal. |
| Malentendidos con el cliente | Estado de cuenta en PDF con detalle de cada movimiento. |
| Cambios de esquema que rompan datos | Migraciones con `PRAGMA user_version`; nunca editar una migración publicada |
| El alcance crece | Todo lo que no esté en la sección 2 va a «Después del MVP» en el [ROADMAP](ROADMAP.md) |

## 10. Criterios para lanzar el MVP (v1.0.0)

- [ ] Todas las historias de la sección 4 cumplen sus criterios de aceptación.
- [ ] Todas las reglas de negocio de la sección 3 tienen pruebas o verificación manual.
- [ ] Probado en un teléfono Android real, en modo avión.
- [ ] APK generado con `npm run build:apk` e instalado en un teléfono limpio.
- [ ] README con capturas de pantalla y CHANGELOG actualizado.
- [ ] Release v1.0.0 publicado con el APK adjunto.

## 11. Glosario

| Término | Significado en CuentaClara |
|---|---|
| `clientes` | Clientes con teléfono y límite de crédito opcional. |
| `movimientos` | Cada fiado o abono con su monto, nota y fecha; los errores se anulan sin borrarse. |
| `ajustes` | Configuración clave-valor (nombre de la tienda, plantilla de mensaje). |
| `movimientos.tipo` | Valores: fiado, abono |

## 12. Mensajes estándar

| Situación | Mensaje |
|---|---|
| Campo obligatorio vacío | Completá este campo |
| Número inválido | Ingresá un número válido |
| Monto cero o negativo | El monto debe ser mayor a 0 |
| Fecha inválida | Fecha no válida |
| Guardado correcto | Guardado |
| Error al guardar | No se pudo guardar. Intentá de nuevo |
| Confirmar borrado | ¿Borrar «{nombre}»? No se puede deshacer |
| Lista vacía | Todavía no hay nada acá. Tocá «+» para empezar |

## 13. Trazabilidad

| Historia | Fase | Casos de prueba |
|---|---|---|
| HU-01 Registrar clientes | 2 | CP-01-1, CP-01-2 |
| HU-02 Registrar fiados y abonos | 2 | CP-02-1, CP-02-2, CP-02-3 |
| HU-03 Anular un movimiento | 2 | CP-03-1 |
| HU-04 Ver saldos y antigüedad | 2 | CP-04-1 |
| HU-05 Estado de cuenta en PDF | 3 | CP-05-1 |
| HU-06 Cobrar por WhatsApp | 4 | CP-06-1, CP-06-2 |
| HU-07 Resumen y respaldo | 5 | CP-07-1, CP-07-2 |

## 14. Documentos relacionados

- [DATOS.md](DATOS.md) · [PANTALLAS.md](PANTALLAS.md) · [PRUEBAS.md](PRUEBAS.md) · [BACKLOG.md](BACKLOG.md) · [ROADMAP.md](ROADMAP.md) · [PLANNING.md](PLANNING.md)
