# Plan de pruebas — CuentaClara

> Qué probar, cuándo y cómo. Cada historia y cada regla de negocio tiene casos concretos. Complementa a [SPEC.md](SPEC.md).

## 1. Estrategia

| Nivel | Herramienta | Qué cubre | Cuándo |
|---|---|---|---|
| Unitarias | jest-expo (`npm test`) | Lógica pura de `src/lib/` (cálculos, validaciones, formatos) | En cada commit (CI) |
| Manuales por historia | Teléfono con Expo Go | Criterios de aceptación de SPEC (casos CP-) | Al cerrar cada tarea de pantalla |
| Regresión por fase | Teléfono real | Lo ya terminado sigue andando | Al cerrar cada fase |
| Aceptación final | APK en teléfono limpio, modo avión | Criterios de lanzamiento | Antes de v1.0.0 |

Meta de cobertura: **100 % de las reglas de negocio** con prueba automática o caso manual, y **100 % de los criterios de aceptación** ejecutados al menos una vez en un teléfono real.

## 2. Pruebas unitarias por regla de negocio

Crear un archivo `src/lib/<tema>.test.ts` por tema. Una prueba por regla como mínimo; agregar los bordes de la sección 4.

| ID | Regla | Tipo de prueba | Casos mínimos |
|---|---|---|---|
| RN-01 | Un movimiento es un fiado (aumenta la deuda) o un abono (la reduce). | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-02 | Saldo del cliente = suma de fiados − suma de abonos, sin contar los movimientos anulados. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-03 | Un abono mayor al saldo se rechaza o se registra como saldo a favor tras confirmación. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-04 | Si el cliente tiene límite, un fiado que lo supere pide confirmación explícita. | Unitaria o manual | Caso válido, caso inválido, límite |
| RN-05 | Los movimientos no se borran: se anulan con una marca y se excluyen del saldo. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-06 | La antigüedad de la deuda cuenta desde el fiado pendiente más antiguo (método primero en entrar, primero en pagar). | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |
| RN-07 | El estado de cuenta lista los movimientos no anulados y el saldo final a la fecha. | Unitaria (cálculo) | Valor típico, cero, redondeo a 2 decimales, valor grande |

## 3. Casos manuales por historia

Formato: ejecutar en el teléfono, marcar ✔ o ✘ y anotar la fecha. Si falla, abrir un issue con el ID del caso.

### HU-01 — Registrar clientes (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-01-1 | no hay clientes | abro la app | veo el estado vacío con «Agregar cliente» | ☐ |
| CP-01-2 | un cliente con teléfono repetido | intento guardarlo | la app avisa y ofrece abrir el existente | ☐ |

Casos transversales: **CP-01-T1** modo avión · **CP-01-T2** cerrar y reabrir la app (persistencia) · **CP-01-T3** texto del sistema en tamaño grande.

### HU-02 — Registrar fiados y abonos (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-02-1 | un cliente con saldo 0 | registro un fiado de 45 Bs | su saldo es 45 Bs y aparece el movimiento con fecha | ☐ |
| CP-02-2 | un cliente con saldo 45 | registro un abono de 20 | su saldo baja a 25 Bs | ☐ |
| CP-02-3 | un fiado que supera el límite del cliente | intento guardarlo | la app pide confirmación | ☐ |

Casos transversales: **CP-02-T1** modo avión · **CP-02-T2** cerrar y reabrir la app (persistencia) · **CP-02-T3** texto del sistema en tamaño grande.

### HU-03 — Anular un movimiento (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-03-1 | un fiado equivocado | lo anulo | queda marcado como anulado, se excluye del saldo y sigue visible en el historial | ☐ |

Casos transversales: **CP-03-T1** modo avión · **CP-03-T2** cerrar y reabrir la app (persistencia) · **CP-03-T3** texto del sistema en tamaño grande.

### HU-04 — Ver saldos y antigüedad (fase 2)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-04-1 | clientes con deudas | abro Clientes | veo la lista ordenada por saldo con los días de antigüedad | ☐ |

Casos transversales: **CP-04-T1** modo avión · **CP-04-T2** cerrar y reabrir la app (persistencia) · **CP-04-T3** texto del sistema en tamaño grande.

### HU-05 — Estado de cuenta en PDF (fase 3)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-05-1 | un cliente con movimientos | toco «Estado de cuenta» | se genera un PDF con movimientos y saldo final y se abre el menú de compartir | ☐ |

Casos transversales: **CP-05-T1** modo avión · **CP-05-T2** cerrar y reabrir la app (persistencia) · **CP-05-T3** texto del sistema en tamaño grande.

### HU-06 — Cobrar por WhatsApp (fase 4)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-06-1 | un cliente con deuda y teléfono | toco «Cobrar» | se abre WhatsApp con su nombre y el saldo | ☐ |
| CP-06-2 | un cliente sin teléfono | veo su ficha | el botón está deshabilitado y sugiere cargar el teléfono | ☐ |

Casos transversales: **CP-06-T1** modo avión · **CP-06-T2** cerrar y reabrir la app (persistencia) · **CP-06-T3** texto del sistema en tamaño grande.

### HU-07 — Resumen y respaldo (fase 5)

| ID | Dado | Cuando | Entonces | Resultado |
|---|---|---|---|---|
| CP-07-1 | clientes con deudas | abro Inicio | veo total por cobrar y los 5 mayores deudores | ☐ |
| CP-07-2 | quiero respaldar | toco «Exportar respaldo» | se genera un archivo que puedo guardar o compartir y luego restaurar en otro teléfono | ☐ |

Casos transversales: **CP-07-T1** modo avión · **CP-07-T2** cerrar y reabrir la app (persistencia) · **CP-07-T3** texto del sistema en tamaño grande.

## 4. Casos borde

| ID | Caso | Resultado esperado | Resultado |
|---|---|---|---|
| CB-01 | Cliente que paga de más: saldo a favor con confirmación. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-02 | Cliente con el mismo nombre: distinguir por teléfono y nota. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-03 | Fecha de un fiado antigua: permitida para cargar el cuaderno. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-04 | Archivo de respaldo de una versión más nueva del esquema: rechazar con mensaje claro. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |
| CB-05 | Cliente dado de baja con saldo: aparece en deudores hasta saldarse. | La app no se rompe; el mensaje o comportamiento es claro y no se pierden datos | ☐ |

## 5. Pruebas de datos y migraciones

| ID | Prueba | Esperado |
|---|---|---|
| CD-01 | Primer arranque en teléfono limpio | La base se crea con todas las tablas y `user_version` queda en 1 |
| CD-02 | Actualizar de una versión anterior con datos cargados | Las migraciones nuevas se aplican y no se pierde ningún registro |
| CD-03 | Borrar un registro padre | Los hijos se borran o se bloquea con mensaje, según el esquema |
| CD-04 | Intentar guardar con campos obligatorios vacíos | No se inserta nada y se marcan los campos |
| CD-05 | Cargar 1.000 registros de prueba | Las listas siguen fluidas (scroll sin trabas) y abren en menos de 2 s |

## 6. Regresión al cerrar cada fase

**Fase 1 — Estructura base y navegación**

- [ ] La app abre y navega sin errores

**Fase 2 — Clientes y movimientos**

- [ ] El saldo coincide con 6 casos de prueba, incluida la anulación.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 3 — Estados de cuenta en PDF**

- [ ] El PDF abre y se comparte desde un teléfono real.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 4 — Recordatorios por WhatsApp**

- [ ] El mensaje llega a WhatsApp con nombre y saldo correctos.
- [ ] Los criterios de las fases anteriores siguen funcionando

**Fase 5 — Respaldo y restauración**

- [ ] Un respaldo restaurado en otro teléfono reproduce exactamente los saldos.
- [ ] Los criterios de las fases anteriores siguen funcionando

## 7. Matriz de dispositivos

| Dispositivo | Android | Qué se verifica |
|---|---|---|
| Gama baja (2–3 GB de RAM) | 8–10 | Fluidez de listas, tiempo de arranque |
| Gama media | 11–13 | Flujo completo y APK |
| Pantalla pequeña (5") | cualquiera | Que nada quede cortado ni oculto por el teclado |
| Texto del sistema grande | cualquiera | Que los formularios sigan siendo usables |

## 8. Lista de verificación para el lanzamiento (v1.0.0)

- [ ] Todas las historias ejecutadas en teléfono real (casos CP-)
- [ ] Todos los casos borde (CB-) ejecutados
- [ ] Pruebas de datos (CD-) ejecutadas
- [ ] APK instalado en un teléfono limpio sin Expo Go
- [ ] Probado en modo avión durante todo el flujo principal
- [ ] Rotación de pantalla no pierde datos del formulario
- [ ] Permisos solicitados solo cuando se necesitan y con explicación
- [ ] Sin errores en la consola durante un recorrido completo
- [ ] README con capturas y CHANGELOG al día

## 9. Informe de errores

```
Título: [Bug] <qué pasó>
ID de caso: CP-XX-X
Pasos: 1... 2... 3...
Esperado: ...
Obtenido: ...
Dispositivo / Android: ...
Captura o video: ...
```
