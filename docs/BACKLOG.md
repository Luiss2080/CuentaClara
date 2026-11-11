# Backlog de issues — CuentaClara

> Lista lista para pasar a GitHub Issues, con milestones, etiquetas, estimaciones y trazabilidad a las historias. Se puede crear todo de una vez con [crear-issues.sh](crear-issues.sh).

## 1. Milestones

| Milestone | Fase | Días | Tareas | Horas est. |
|---|---|---|---|---|
| Fase 1 — Estructura base y navegación | 1 | 3 | 8 | 20 |
| Fase 2 — Clientes y movimientos | 2 | 5 | 8 | 32 |
| Fase 3 — Estados de cuenta en PDF | 3 | 4 | 7 | 24.5 |
| Fase 4 — Recordatorios por WhatsApp | 4 | 3 | 7 | 17.5 |
| Fase 5 — Respaldo y restauración | 5 | 4 | 8 | 24 |

Total: **38 tareas**, ~118 horas (19 días a 6 horas efectivas).

## 2. Etiquetas

| Etiqueta | Uso |
|---|---|
| ui | Pantallas y componentes |
| datos | Tablas, consultas y migraciones |
| lógica | Reglas de negocio y cálculos |
| pruebas | Pruebas automáticas y manuales |
| integración | Cámara, notificaciones, PDF, compartir |
| tarea | Otras tareas |
| bug | Errores |
| mejora | Ideas para v1.1+ |

## 3. Issues del MVP

### Fase 1 — Estructura base y navegación

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F1-T1` | Crear el proyecto Expo con TypeScript y Expo Router | — | tarea | Alta | 2.5 |
| `F1-T2` | Alinear versiones y herramientas con los repos hermanos | — | tarea | Alta | 2.5 |
| `F1-T3` | Crear la estructura de carpetas | — | tarea | Alta | 2.5 |
| `F1-T4` | Implementar apertura de base y migración 1 | — | datos | Alta | 2.5 |
| `F1-T5` | Definir la navegación principal | — | ui | Alta | 2.5 |
| `F1-T6` | Agregar componente Pantalla, tema e inicio | — | ui | Alta | 2.5 |
| `F1-T7` | Configurar scripts y CI | — | tarea | Alta | 2.5 |
| `F1-T8` | Primera prueba de src/lib | — | pruebas | Alta | 2.5 |

### Fase 2 — Clientes y movimientos

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F2-T1` | CRUD de clientes con búsqueda y validación de teléfono. | HU-01, HU-02, HU-03, HU-04 | lógica | Alta | 4 |
| `F2-T2` | Registro de fiados y abonos con validaciones y límite. | HU-01, HU-02, HU-03, HU-04 | lógica | Alta | 4 |
| `F2-T3` | Anulación de movimientos sin borrar. | HU-01, HU-02, HU-03, HU-04 | tarea | Alta | 4 |
| `F2-T4` | Función pura de saldo y antigüedad con pruebas (incluye anulados y abonos parciales). | HU-01, HU-02, HU-03, HU-04 | lógica, pruebas | Alta | 4 |
| `F2-T5` | Pantalla Inicio con total por cobrar y mayores deudores. | HU-01, HU-02, HU-03, HU-04 | ui | Alta | 4 |
| `F2-T6` | Validaciones, estados vacíos y mensajes en español | HU-01, HU-02, HU-03, HU-04 | lógica | Alta | 4 |
| `F2-T7` | Pruebas automáticas y verificación manual de criterios | HU-01, HU-02, HU-03, HU-04 | pruebas | Alta | 4 |
| `F2-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-01, HU-02, HU-03, HU-04 | pruebas | Alta | 4 |

### Fase 3 — Estados de cuenta en PDF

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F3-T1` | Datos de la tienda en Ajustes. | HU-05 | tarea | Alta | 3.5 |
| `F3-T2` | Plantilla HTML del estado de cuenta. | HU-05 | tarea | Alta | 3.5 |
| `F3-T3` | Generación con expo-print y compartir con expo-sharing. | HU-05 | integración | Alta | 3.5 |
| `F3-T4` | Rango de fechas opcional en el estado de cuenta. | HU-05 | tarea | Alta | 3.5 |
| `F3-T5` | Validaciones, estados vacíos y mensajes en español | HU-05 | lógica | Alta | 3.5 |
| `F3-T6` | Pruebas automáticas y verificación manual de criterios | HU-05 | pruebas | Alta | 3.5 |
| `F3-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-05 | pruebas | Alta | 3.5 |

### Fase 4 — Recordatorios por WhatsApp

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F4-T1` | Plantilla de cobro editable con {nombre} y {saldo}. | HU-06 | tarea | Media | 2.5 |
| `F4-T2` | Enlace wa.me con texto codificado y normalización +591. | HU-06 | tarea | Media | 2.5 |
| `F4-T3` | Lista «a quién cobrar hoy» ordenada por antigüedad. | HU-06 | ui | Media | 2.5 |
| `F4-T4` | Manejo de clientes sin teléfono. | HU-06 | tarea | Media | 2.5 |
| `F4-T5` | Validaciones, estados vacíos y mensajes en español | HU-06 | lógica | Media | 2.5 |
| `F4-T6` | Pruebas automáticas y verificación manual de criterios | HU-06 | pruebas | Media | 2.5 |
| `F4-T7` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-06 | pruebas | Media | 2.5 |

### Fase 5 — Respaldo y restauración

| ID | Título | Historias | Etiquetas | Prioridad | Horas |
|---|---|---|---|---|---|
| `F5-T1` | Exportar el archivo de base de datos o un JSON con todos los datos con expo-file-system. | HU-07 | datos, integración | Media | 3 |
| `F5-T2` | Compartir el respaldo con expo-sharing. | HU-07 | integración | Media | 3 |
| `F5-T3` | Restaurar desde un archivo con validación de versión de esquema. | HU-07 | datos, lógica | Media | 3 |
| `F5-T4` | Confirmaciones claras antes de sobrescribir datos. | HU-07 | tarea | Media | 3 |
| `F5-T5` | Pruebas de ida y vuelta (exportar y restaurar da los mismos datos). | HU-07 | pruebas, integración | Media | 3 |
| `F5-T6` | Validaciones, estados vacíos y mensajes en español | HU-07 | lógica | Media | 3 |
| `F5-T7` | Pruebas automáticas y verificación manual de criterios | HU-07 | pruebas | Media | 3 |
| `F5-T8` | Prueba en teléfono real en modo avión; actualizar ROADMAP y CHANGELOG | HU-07 | pruebas | Media | 3 |

## 4. Orden recomendado de ataque

1. Terminá cada fase completa antes de abrir la siguiente; dentro de la fase, de arriba hacia abajo.
2. Las tareas `lógica` y `datos` van antes que las `ui` que las usan.
3. Cada issue se cierra con un commit o PR que lo mencione (`Cierra #N`).
4. Nada de abrir issues de v1.1 hasta tener el APK de la v1.0.0.

## 5. Ideas para después del MVP

| Idea | Esfuerzo | Valor |
|---|---|---|
| Modo oscuro | Bajo | Medio |
| Búsqueda y filtros avanzados | Medio | Alto |
| Estadísticas y gráficos simples en inicio | Medio | Medio |
| Respaldo y restauración de la base | Medio | Alto |
| Publicación en Google Play | Bajo | Alto |
| Sincronización opcional entre dispositivos | Alto | Depende de demanda |
