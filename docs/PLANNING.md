# Plan de desarrollo — CuentaClara

Guía para construir el MVP **fase por fase**. Complementa a [ROADMAP.md](ROADMAP.md) (qué y cuándo) y [SPEC.md](SPEC.md) (qué debe hacer la app).

## Forma de trabajo

1. **Una fase por rama:** `git checkout -b fase-N-nombre`.
2. **Un issue por tarea** (usá los títulos de abajo) y un milestone por fase en GitHub.
3. **Commits chicos** con prefijo: `feat:`, `fix:`, `test:`, `docs:`, `chore:`.
4. **Pull request al cerrar la fase** con la lista de criterios de salida marcada.
5. **Primero la lógica pura y sus pruebas** (en `src/lib/`), después la pantalla que la usa.
6. **Probá en el teléfono** al terminar cada tarea de pantalla, no solo al final de la fase.

### Definición de «terminado» (vale para toda tarea y fase)

- [ ] `npm run lint`, `npx tsc --noEmit` y `npm test` sin errores.
- [ ] Funciona en modo avión.
- [ ] Estados vacíos, de carga y de error resueltos, con textos en español.
- [ ] Los datos persisten al reiniciar la app.
- [ ] La regla de negocio correspondiente tiene prueba o verificación manual anotada.
- [ ] CHANGELOG actualizado al cerrar la fase.

## Calendario sugerido

| Fase | Nombre | Días | Acumulado |
|---|---|---|---|
| 1 | Estructura base y navegación | 3 | 3 |
| 2 | Clientes y movimientos | 5 | 8 |
| 3 | Estados de cuenta en PDF | 4 | 12 |
| 4 | Recordatorios por WhatsApp | 3 | 15 |
| 5 | Respaldo y restauración | 4 | 19 |

Total: **19 días** (~4 semanas). Si una fase se pasa del doble de lo estimado, recortá alcance dentro de la fase en lugar de saltar a la siguiente.

---

## Fase 1 — Estructura base y navegación

**Estimación:** 3 días.

**Objetivo:** crear el proyecto Expo con base de datos, navegación, tema y CI. Este repositorio hoy solo tiene documentación; la fase 1 agrega el código base.

**Tareas**

- [ ] **F1-T1** Crear el proyecto en una carpeta temporal con `npx create-expo-app@latest` (plantilla con TypeScript y Expo Router) y copiar su contenido a este repositorio sin pisar README, LICENSE ni los archivos de comunidad.
- [ ] **F1-T2** Alinear versiones y herramientas con los repositorios hermanos (por ejemplo PasanakuYa): Expo SDK 58, `expo-sqlite`, `jest-expo`, ESLint, TypeScript estricto, `.editorconfig` y `.prettierrc`.
- [ ] **F1-T3** Crear la estructura `app/`, `src/db/`, `src/components/`, `src/lib/` y `src/theme.ts`.
- [ ] **F1-T4** Implementar `src/db/index.ts` (apertura de la base) y `src/db/schema.ts` con la migración 1 tomada de la sección 6 del [SPEC](SPEC.md).
- [ ] **F1-T5** Definir la navegación principal en `app/_layout.tsx` con una ruta por sección.
- [ ] **F1-T6** Agregar el componente `Pantalla`, el tema y la pantalla de inicio con accesos a cada sección.
- [ ] **F1-T7** Configurar scripts `lint`, `test`, `build:apk` y el workflow de GitHub Actions (`.github/workflows/ci.yml`).
- [ ] **F1-T8** Escribir una primera prueba de `src/lib` para validar que Jest funciona.

**Criterio de salida:** la app abre, navega entre secciones vacías y no da errores; lint, tipos y tests en verde.

---

## Fase 2 — Clientes y movimientos

**Estimación:** 5 días. **Rama sugerida:** `fase-2-clientes-y-movimientos`.

**Objetivo:** Clientes, fiados y abonos.

**Historias que se completan:** HU-01 (Registrar clientes), HU-02 (Registrar fiados y abonos), HU-03 (Anular un movimiento), HU-04 (Ver saldos y antigüedad).

**Tareas**

- [ ] **F2-T1** CRUD de clientes con búsqueda y validación de teléfono.
- [ ] **F2-T2** Registro de fiados y abonos con validaciones y límite.
- [ ] **F2-T3** Anulación de movimientos sin borrar.
- [ ] **F2-T4** Función pura de saldo y antigüedad con pruebas (incluye anulados y abonos parciales).
- [ ] **F2-T5** Pantalla Inicio con total por cobrar y mayores deudores.
- [ ] **F2-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F2-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F2-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-01 — no hay clientes → abro la app → veo el estado vacío con «Agregar cliente».
- [ ] HU-01 — un cliente con teléfono repetido → intento guardarlo → la app avisa y ofrece abrir el existente.
- [ ] HU-02 — un cliente con saldo 0 → registro un fiado de 45 Bs → su saldo es 45 Bs y aparece el movimiento con fecha.
- [ ] HU-02 — un cliente con saldo 45 → registro un abono de 20 → su saldo baja a 25 Bs.
- [ ] HU-02 — un fiado que supera el límite del cliente → intento guardarlo → la app pide confirmación.
- [ ] HU-03 — un fiado equivocado → lo anulo → queda marcado como anulado, se excluye del saldo y sigue visible en el historial.
- [ ] HU-04 — clientes con deudas → abro Clientes → veo la lista ordenada por saldo con los días de antigüedad.

**Criterio de salida:**

- [ ] El saldo coincide con 6 casos de prueba, incluida la anulación.
- [ ] Definición de «terminado» cumplida.

---

## Fase 3 — Estados de cuenta en PDF

**Estimación:** 4 días. **Rama sugerida:** `fase-3-estados-de-cuenta-en-pdf`.

**Objetivo:** Estados de cuenta en PDF.

**Historias que se completan:** HU-05 (Estado de cuenta en PDF).

**Tareas**

- [ ] **F3-T1** Datos de la tienda en Ajustes.
- [ ] **F3-T2** Plantilla HTML del estado de cuenta.
- [ ] **F3-T3** Generación con expo-print y compartir con expo-sharing.
- [ ] **F3-T4** Rango de fechas opcional en el estado de cuenta.
- [ ] **F3-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F3-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F3-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-05 — un cliente con movimientos → toco «Estado de cuenta» → se genera un PDF con movimientos y saldo final y se abre el menú de compartir.

**Criterio de salida:**

- [ ] El PDF abre y se comparte desde un teléfono real.
- [ ] Definición de «terminado» cumplida.

---

## Fase 4 — Recordatorios por WhatsApp

**Estimación:** 3 días. **Rama sugerida:** `fase-4-recordatorios-por-whatsapp`.

**Objetivo:** Recordatorios por WhatsApp.

**Historias que se completan:** HU-06 (Cobrar por WhatsApp).

**Tareas**

- [ ] **F4-T1** Plantilla de cobro editable con {nombre} y {saldo}.
- [ ] **F4-T2** Enlace wa.me con texto codificado y normalización +591.
- [ ] **F4-T3** Lista «a quién cobrar hoy» ordenada por antigüedad.
- [ ] **F4-T4** Manejo de clientes sin teléfono.
- [ ] **F4-T5** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F4-T6** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F4-T7** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-06 — un cliente con deuda y teléfono → toco «Cobrar» → se abre WhatsApp con su nombre y el saldo.
- [ ] HU-06 — un cliente sin teléfono → veo su ficha → el botón está deshabilitado y sugiere cargar el teléfono.

**Criterio de salida:**

- [ ] El mensaje llega a WhatsApp con nombre y saldo correctos.
- [ ] Definición de «terminado» cumplida.

---

## Fase 5 — Respaldo y restauración

**Estimación:** 4 días. **Rama sugerida:** `fase-5-respaldo-y-restauracion`.

**Objetivo:** Respaldo y restauración.

**Historias que se completan:** HU-07 (Resumen y respaldo).

**Tareas**

- [ ] **F5-T1** Exportar el archivo de base de datos o un JSON con todos los datos con expo-file-system.
- [ ] **F5-T2** Compartir el respaldo con expo-sharing.
- [ ] **F5-T3** Restaurar desde un archivo con validación de versión de esquema.
- [ ] **F5-T4** Confirmaciones claras antes de sobrescribir datos.
- [ ] **F5-T5** Pruebas de ida y vuelta (exportar y restaurar da los mismos datos).
- [ ] **F5-T6** Validaciones, estados vacíos y mensajes de error en español para todo lo nuevo.
- [ ] **F5-T7** Pruebas automáticas de la lógica de la fase y verificación manual de los criterios de abajo.
- [ ] **F5-T8** Probar en un teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

**Pruebas de aceptación manuales** (de [SPEC.md](SPEC.md)):

- [ ] HU-07 — clientes con deudas → abro Inicio → veo total por cobrar y los 5 mayores deudores.
- [ ] HU-07 — quiero respaldar → toco «Exportar respaldo» → se genera un archivo que puedo guardar o compartir y luego restaurar en otro teléfono.

**Criterio de salida:**

- [ ] Un respaldo restaurado en otro teléfono reproduce exactamente los saldos.
- [ ] Definición de «terminado» cumplida.

---

## Lanzamiento del MVP (v1.0.0)

- [ ] Revisar todas las historias de [SPEC.md](SPEC.md) y marcar sus criterios.
- [ ] Capturas de pantalla para el README.
- [ ] Revisar textos, íconos y nombre de la app en `app.json`.
- [ ] `npm run build:apk` y probar el APK en un teléfono limpio, sin Expo Go y en modo avión.
- [ ] Publicar el release v1.0.0 con el APK adjunto.
- [ ] Pasar las ideas pendientes a issues para la versión 1.1.

## Plantilla de issue

Copiá esto al crear cada issue:

```
Título: [F2-T1] <descripción de la tarea>
Fase: N — <nombre>
Historia: HU-XX
Criterios:
- [ ] ...
Notas técnicas: ...
```

---

## Cronograma día por día

Guía para no perder el hilo: cada día tiene tareas concretas. Si un día se atrasa, mové la tarea al siguiente, no la saltees.

### Fase 1 — Estructura base y navegación

- **Día 1** (día 1 de la fase): `F1-T1` Crear el proyecto Expo con TypeScript y Expo Router; `F1-T2` Alinear versiones y herramientas con los repos hermanos; `F1-T3` Crear la estructura de carpetas.
- **Día 2** (día 2 de la fase): `F1-T4` Implementar apertura de base y migración 1; `F1-T5` Definir la navegación principal; `F1-T6` Agregar componente Pantalla, tema e inicio.
- **Día 3** (día 3 de la fase): `F1-T7` Configurar scripts y CI; `F1-T8` Primera prueba de src/lib.

### Fase 2 — Clientes y movimientos

- **Día 4** (día 1 de la fase): `F2-T1` CRUD de clientes con búsqueda y validación de teléfono.; `F2-T2` Registro de fiados y abonos con validaciones y límite..
- **Día 5** (día 2 de la fase): `F2-T3` Anulación de movimientos sin borrar.; `F2-T4` Función pura de saldo y antigüedad con pruebas (incluye anulados y abonos parciales)..
- **Día 6** (día 3 de la fase): `F2-T5` Pantalla Inicio con total por cobrar y mayores deudores.; `F2-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 7** (día 4 de la fase): `F2-T7` Pruebas automáticas y verificación manual de criterios; `F2-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.
- **Día 8** (día 5 de la fase): colchón: terminar pendientes, probar en el teléfono y ajustar detalles.

### Fase 3 — Estados de cuenta en PDF

- **Día 9** (día 1 de la fase): `F3-T1` Datos de la tienda en Ajustes.; `F3-T2` Plantilla HTML del estado de cuenta..
- **Día 10** (día 2 de la fase): `F3-T3` Generación con expo-print y compartir con expo-sharing.; `F3-T4` Rango de fechas opcional en el estado de cuenta..
- **Día 11** (día 3 de la fase): `F3-T5` Validaciones, estados vacíos y mensajes en español; `F3-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 12** (día 4 de la fase): `F3-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 4 — Recordatorios por WhatsApp

- **Día 13** (día 1 de la fase): `F4-T1` Plantilla de cobro editable con {nombre} y {saldo}.; `F4-T2` Enlace wa.me con texto codificado y normalización +591.; `F4-T3` Lista «a quién cobrar hoy» ordenada por antigüedad..
- **Día 14** (día 2 de la fase): `F4-T4` Manejo de clientes sin teléfono.; `F4-T5` Validaciones, estados vacíos y mensajes en español; `F4-T6` Pruebas automáticas y verificación manual de criterios.
- **Día 15** (día 3 de la fase): `F4-T7` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

### Fase 5 — Respaldo y restauración

- **Día 16** (día 1 de la fase): `F5-T1` Exportar el archivo de base de datos o un JSON con todos los datos con expo-file-system.; `F5-T2` Compartir el respaldo con expo-sharing..
- **Día 17** (día 2 de la fase): `F5-T3` Restaurar desde un archivo con validación de versión de esquema.; `F5-T4` Confirmaciones claras antes de sobrescribir datos..
- **Día 18** (día 3 de la fase): `F5-T5` Pruebas de ida y vuelta (exportar y restaurar da los mismos datos).; `F5-T6` Validaciones, estados vacíos y mensajes en español.
- **Día 19** (día 4 de la fase): `F5-T7` Pruebas automáticas y verificación manual de criterios; `F5-T8` Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG.

## Rutina diaria

1. Empezá releyendo la tarea del día en [BACKLOG.md](BACKLOG.md) y su regla o historia en [SPEC.md](SPEC.md).
2. Escribí primero la prueba de la lógica, después el código.
3. Probá en el teléfono antes de hacer commit.
4. Cerrá el día con un commit, el issue marcado y una línea en el CHANGELOG si corresponde.

## Qué hacer si te trabás

- Más de 2 horas sin avanzar: simplificá la tarea al caso mínimo y anotá lo que quedó fuera como issue `mejora`.
- Duda de alcance: la respuesta está en la sección 2 del [SPEC](SPEC.md); si no está, no entra al MVP.
- Duda de datos o validaciones: [DATOS.md](DATOS.md).
- Duda de pantallas: [PANTALLAS.md](PANTALLAS.md).
- Duda de cómo probar: [PRUEBAS.md](PRUEBAS.md).
