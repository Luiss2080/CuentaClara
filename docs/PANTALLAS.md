# Pantallas y flujos — CuentaClara

> Guía de interfaz para construir cada pantalla: ruta, contenido, estados y bosquejo. Complementa la sección 5 de [SPEC.md](SPEC.md).

## 1. Mapa de rutas

| Pantalla | Archivo (Expo Router) | Tipo |
|---|---|---|
| Inicio | `app/index.tsx` | Inicio |
| Clientes | `app/clientes.tsx` | Lista / vista |
| Ficha de cliente | `app/ficha-de-cliente.tsx` | Detalle |
| Formulario de movimiento | `app/formulario-de-movimiento.tsx` | Formulario |
| Ajustes | `app/ajustes.tsx` | Lista / vista |
| Respaldo | `app/respaldo.tsx` | Lista / vista |

**Convenciones de interfaz**

- Una acción principal por pantalla, en un botón grande abajo o en un botón flotante.
- Los formularios se abren como pantalla completa, validan al enviar y muestran el error junto al campo.
- Toda lista tiene estado vacío con una frase y un botón que lleva a la acción.
- Las acciones que borran piden confirmación explícita con el nombre del registro.
- El botón atrás nunca pierde datos sin avisar: si el formulario tiene cambios, se pregunta antes de salir.
- Textos en español de Bolivia (voseo opcional, siempre claros); montos como `Bs 1.250,00`.

## 2. Detalle por pantalla

### 2.1 Inicio

- **Archivo:** `app/index.tsx`
- **Qué muestra:** Total por cobrar y mayores deudores
- **Acciones:** Registrar fiado o abono

**Bosquejo**

```
┌──────────────────────────────┐
│ CuentaClara                  │
├──────────────────────────────┤
│ [ Resumen y alertas ]        │
│                              │
│  ▸ Clientes                 │
│  ▸ Ficha de cliente         │
│  ▸ Ajustes                  │
│  ▸ Respaldo                 │
│                              │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Total por cobrar y mayores deudores |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.2 Clientes

- **Archivo:** `app/clientes.tsx`
- **Qué muestra:** Lista con saldo y antigüedad, búsqueda
- **Acciones:** Crear, abrir ficha
- **Historias relacionadas:** HU-01, HU-04, HU-05 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Clientes                    │
├──────────────────────────────┤
│ nombre · limite              │
│ nombre · limite              │
│ nombre · limite              │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay clientes. Tocá «Crear» para empezar. |
| Con datos | Lista con saldo y antigüedad, búsqueda |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.3 Ficha de cliente

- **Archivo:** `app/ficha-de-cliente.tsx`
- **Qué muestra:** Saldo, historial de movimientos y datos
- **Acciones:** Fiado, abono, estado de cuenta, cobrar por WhatsApp
- **Historias relacionadas:** HU-01, HU-02, HU-03, HU-04, HU-05, HU-06, HU-07 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Ficha de cliente            │
├──────────────────────────────┤
│ nombre                       │
│ Resumen / totales            │
├──────────────────────────────┤
│ • registro 1                 │
│ • registro 2                 │
│ • registro 3                 │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay clientes. Tocá «Fiado» para empezar. |
| Con datos | Saldo, historial de movimientos y datos |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.4 Formulario de movimiento

- **Archivo:** `app/formulario-de-movimiento.tsx`
- **Qué muestra:** Tipo, monto, nota, fecha
- **Acciones:** Guardar
- **Historias relacionadas:** HU-03 (sugerido por coincidencia de temas; confirmar al construir).

**Campos del formulario**

| Campo | Control | Obligatorio | Validación |
|---|---|---|---|
| cliente id | Selector de clientes | Sí | Debe existir en `clientes` |
| tipo | Selector / fichas | Sí | Uno de: `fiado`, `abono` |
| monto | Numérico decimal | Sí | Mayor a 0; Hasta 2 decimales |
| nota | Texto largo | No | Hasta 280 caracteres |
| fecha | Fecha | Sí | Fecha real en formato AAAA-MM-DD |
| anulado | Interruptor | Sí | 0 o 1 |

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Formulario de movimiento    │
├──────────────────────────────┤
│ cliente id                   │
│ [____________________]       │
│ tipo                         │
│ [____________________]       │
│ monto                        │
│ [____________________]       │
│ nota                         │
│ [____________________]       │
│ fecha                        │
│ [____________________]       │
│                              │
│ [        Guardar        ]    │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay movimientos. Tocá «Guardar» para empezar. |
| Con datos | Tipo, monto, nota, fecha |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.5 Ajustes

- **Archivo:** `app/ajustes.tsx`
- **Qué muestra:** Nombre de la tienda, plantilla de cobro
- **Acciones:** Guardar
- **Historias relacionadas:** HU-06 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Ajustes                     │
├──────────────────────────────┤
│ valor · fecha                │
│ valor · fecha                │
│ valor · fecha                │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay ajustes. Tocá «Guardar» para empezar. |
| Con datos | Nombre de la tienda, plantilla de cobro |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

### 2.6 Respaldo

- **Archivo:** `app/respaldo.tsx`
- **Qué muestra:** Exportar y restaurar el archivo de datos
- **Acciones:** Exportar, restaurar
- **Historias relacionadas:** HU-07 (sugerido por coincidencia de temas; confirmar al construir).

**Bosquejo**

```
┌──────────────────────────────┐
│ ←  Respaldo                    │
├──────────────────────────────┤
│ nombre · fecha               │
│ nombre · fecha               │
│ nombre · fecha               │
│                              │
│                         (+)  │
└──────────────────────────────┘
```

**Estados**

| Estado | Qué ve el usuario |
|---|---|
| Cargando | Indicador breve; la base es local, rara vez se nota |
| Vacío | Todavía no hay información para mostrar. |
| Con datos | Exportar y restaurar el archivo de datos |
| Error | Mensaje «No se pudo guardar/cargar. Intentá de nuevo» con botón de reintento; el error técnico va a la consola |
| Sin cambios (formularios) | El botón guardar queda deshabilitado hasta que haya cambios válidos |

**Accesibilidad:** cada botón con `accessibilityLabel`, áreas táctiles de 48 px o más, no depender solo del color para indicar estado.

## 3. Flujos principales

**HU-01 — Registrar clientes**

```
abro la app  →  veo el estado vacío con «Agregar cliente»
```

**HU-02 — Registrar fiados y abonos**

```
registro un fiado de 45 Bs  →  su saldo es 45 Bs y aparece el movimiento con fecha
```

**HU-03 — Anular un movimiento**

```
lo anulo  →  queda marcado como anulado, se excluye del saldo y sigue visible en el historial
```

**HU-04 — Ver saldos y antigüedad**

```
abro Clientes  →  veo la lista ordenada por saldo con los días de antigüedad
```

**HU-05 — Estado de cuenta en PDF**

```
toco «Estado de cuenta»  →  se genera un PDF con movimientos y saldo final y se abre el menú de compartir
```

**HU-06 — Cobrar por WhatsApp**

```
toco «Cobrar»  →  se abre WhatsApp con su nombre y el saldo
```

**HU-07 — Resumen y respaldo**

```
abro Inicio  →  veo total por cobrar y los 5 mayores deudores
```
