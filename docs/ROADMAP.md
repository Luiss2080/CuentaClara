# Roadmap de CuentaClara

> Que el tendero sepa exactamente quién le debe, cuánto y desde cuándo, y pueda cobrar sin discutir.

Ruta de desarrollo del MVP, fase por fase. Cada fase termina con algo que se puede **usar y probar en un teléfono**. Trabajá una fase a la vez y no pases a la siguiente hasta cumplir su criterio de salida.

## Resumen

- **Objetivo del MVP:** Libreta digital de fiados para tiendas de barrio: quién debe, cuánto y desde cuándo, con estados de cuenta en PDF.
- **Usuario:** Dueño de una tienda de barrio con 20 a 300 clientes que compran a crédito.
- **Duración estimada:** ~19 días de trabajo efectivo (~4 semanas a ritmo tranquilo).
- **Costo:** $0 (sin servidores, sin suscripciones, todo offline en SQLite).

## Línea de tiempo

| Fase | Nombre | Objetivo | Días | Semana acumulada |
|---|---|---|---|---|
| 1 | Estructura base y navegación | Crear el proyecto Expo con base de datos, navegación, tema y CI. | 3 | 1 |
| 2 | Clientes y movimientos | Clientes, fiados y abonos. | 5 | 2 |
| 3 | Estados de cuenta en PDF | Estados de cuenta en PDF. | 4 | 3 |
| 4 | Recordatorios por WhatsApp | Recordatorios por WhatsApp. | 3 | 3 |
| 5 | Respaldo y restauración | Respaldo y restauración. | 4 | 4 |

## Hitos

### Hito 1 — Estructura base y navegación

**Demo:** La app abre en Expo Go, navega entre secciones vacías y el CI pasa en verde.

### Hito 2 — Clientes y movimientos

**Demo:** El saldo coincide con 6 casos de prueba, incluida la anulación.

### Hito 3 — Estados de cuenta en PDF

**Demo:** El PDF abre y se comparte desde un teléfono real.

### Hito 4 — Recordatorios por WhatsApp

**Demo:** El mensaje llega a WhatsApp con nombre y saldo correctos.

### Hito 5 — Respaldo y restauración

**Demo:** Un respaldo restaurado en otro teléfono reproduce exactamente los saldos.

## Detalle por fase

### Fase 1 — Estructura base y navegación

- **Objetivo:** dejar el proyecto creado y funcionando con la base de datos y la navegación.
- **Historias cubiertas:** ninguna (infraestructura).
- **Depende de:** nada.
- **Criterio de salida:** la app abre, navega y el CI está en verde.

### Fase 2 — Clientes y movimientos

- **Objetivo:** Clientes, fiados y abonos.
- **Historias cubiertas:** HU-01 Registrar clientes; HU-02 Registrar fiados y abonos; HU-03 Anular un movimiento; HU-04 Ver saldos y antigüedad
- **Depende de:** fase 1.
- **Duración:** 5 días.
- **Criterio de salida:**
  - El saldo coincide con 6 casos de prueba, incluida la anulación.

### Fase 3 — Estados de cuenta en PDF

- **Objetivo:** Estados de cuenta en PDF.
- **Historias cubiertas:** HU-05 Estado de cuenta en PDF
- **Depende de:** fase 2.
- **Duración:** 4 días.
- **Criterio de salida:**
  - El PDF abre y se comparte desde un teléfono real.

### Fase 4 — Recordatorios por WhatsApp

- **Objetivo:** Recordatorios por WhatsApp.
- **Historias cubiertas:** HU-06 Cobrar por WhatsApp
- **Depende de:** fase 3.
- **Duración:** 3 días.
- **Criterio de salida:**
  - El mensaje llega a WhatsApp con nombre y saldo correctos.

### Fase 5 — Respaldo y restauración

- **Objetivo:** Respaldo y restauración.
- **Historias cubiertas:** HU-07 Resumen y respaldo
- **Depende de:** fase 4.
- **Duración:** 4 días.
- **Criterio de salida:**
  - Un respaldo restaurado en otro teléfono reproduce exactamente los saldos.

## Estado

- [ ] **Fase 1:** Estructura base y navegación
- [ ] **Fase 2:** Clientes y movimientos
- [ ] **Fase 3:** Estados de cuenta en PDF
- [ ] **Fase 4:** Recordatorios por WhatsApp
- [ ] **Fase 5:** Respaldo y restauración

## Dependencias y supuestos

- Un teléfono Android real para probar (Expo Go) y, al final, para instalar el APK.
- Node.js 20+ y la app Expo Go.
- Todo lo demás (cuentas, nube, pagos) queda fuera del MVP.

## Después del MVP (v1.1 en adelante)

- [ ] Modo oscuro.
- [ ] Búsqueda y filtros avanzados en las listas.
- [ ] Estadísticas y gráficos simples en la pantalla de inicio.
- [ ] Publicación en Google Play (cuenta de desarrollador, pago único de USD 25).
- [ ] Sincronización opcional entre dispositivos (solo si hay demanda real).

## Cómo usar este documento

1. [SPEC.md](SPEC.md) define **qué** debe hacer la app (historias, reglas, pantallas, datos).
2. [PLANNING.md](PLANNING.md) define **cómo** construirla fase por fase (tareas, pruebas, issues).
3. Al cerrar una fase: marcá el estado acá, actualizá el [CHANGELOG](../CHANGELOG.md) y fusioná la rama a `main`.

## Trazabilidad: historias por fase

| Historia | Fase | Entrega |
|---|---|---|
| HU-01 Registrar clientes | 2 | Clientes y movimientos |
| HU-02 Registrar fiados y abonos | 2 | Clientes y movimientos |
| HU-03 Anular un movimiento | 2 | Clientes y movimientos |
| HU-04 Ver saldos y antigüedad | 2 | Clientes y movimientos |
| HU-05 Estado de cuenta en PDF | 3 | Estados de cuenta en PDF |
| HU-06 Cobrar por WhatsApp | 4 | Recordatorios por WhatsApp |
| HU-07 Resumen y respaldo | 5 | Respaldo y restauración |

## Indicadores de avance

| Indicador | Cómo se mide | Meta |
|---|---|---|
| Avance de tareas | Issues cerrados / 38 | 100 % al lanzar |
| Historias aceptadas | Casos CP- marcados ✔ / 12 | 100 % |
| Reglas cubiertas | RN con prueba / 7 | 100 % |
| Ritmo | Días reales vs estimados por fase | Desvío menor al 50 % |
| Calidad | Errores abiertos al cierre de fase | 0 críticos |

## Revisión de fin de fase

Antes de pasar a la siguiente fase respondé por escrito (5 minutos):

1. ¿Se cumplieron todos los criterios de salida?
2. ¿Cuántos días tomó realmente frente a lo estimado?
3. ¿Qué se recortó o se dejó para después?
4. ¿Hay algo que cambie el SPEC? Actualizalo antes de seguir.

## Documentos de apoyo

- [DATOS.md](DATOS.md): campos, validaciones, consultas y migraciones.
- [PANTALLAS.md](PANTALLAS.md): ruta, estados y bosquejo de cada pantalla.
- [PRUEBAS.md](PRUEBAS.md): casos de prueba por regla, historia y borde.
- [BACKLOG.md](BACKLOG.md): issues listos para GitHub con milestones y etiquetas.
