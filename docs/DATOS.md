# Modelo de datos — CuentaClara

> Diseño detallado de la base SQLite: campos, validaciones, consultas y migraciones. Complementa la sección 6 de [SPEC.md](SPEC.md).

## 1. Convenciones

- Base local `SQLite` con `expo-sqlite`; versión del esquema en `PRAGMA user_version`.
- Claves primarias `INTEGER AUTOINCREMENT` llamadas `id`.
- Fechas como texto ISO `AAAA-MM-DD` (y `AAAA-MM-DDTHH:MM` cuando hay hora). Nunca se guardan textos como «hoy».
- Montos como `REAL` en bolivianos; se redondean a 2 decimales al guardar y se formatean al mostrar.
- Booleanos como `INTEGER` 0/1.
- Los textos se guardan sin espacios sobrantes (`trim()`).
- Todas las consultas usan parámetros (`?`), nunca concatenan texto del usuario.
- Las claves foráneas se activan con `PRAGMA foreign_keys = ON` al abrir la base.

## 2. Relaciones

| Campo | Apunta a | Comportamiento |
|---|---|---|
| `movimientos.cliente_id` | `clientes` | Sin borrado en cascada |

```
clientes  ──<  movimientos
movimientos
ajustes
```

## 3. Detalle por tabla

### 3.1 `clientes`

Clientes con teléfono y límite de crédito opcional.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `nombre` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí nombre» |
| `telefono` | TEXT → `string \| null` | Teléfono | Opcional; Solo dígitos, 7 a 15 caracteres (se ignoran espacios y guiones) | «Teléfono no válido» |
| `limite` | REAL → `number \| null` | Numérico decimal | Opcional; Mayor o igual a 0; Hasta 2 decimales | «Ingresá un número válido» |
| `activo` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `1` | — |

**Consultas base** (en `src/db/clientes.ts`):

```ts
listar(): Promise<Cliente[]>            // SELECT * FROM clientes ORDER BY id DESC
obtener(id: number): Promise<Cliente | null>   // SELECT * FROM clientes WHERE id = ?
crear(datos: NuevoCliente): Promise<number>   // INSERT INTO clientes (nombre, telefono, limite, activo) VALUES (?, ?, ?, ?)
actualizar(id: number, datos: NuevoCliente): Promise<void>   // UPDATE clientes SET nombre = ?, telefono = ?, limite = ?, activo = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM clientes WHERE id = ?
```

### 3.2 `movimientos`

Cada fiado o abono con su monto, nota y fecha; los errores se anulan sin borrarse.

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `id` | INTEGER | Automático | Clave primaria | — |
| `cliente_id` | INTEGER → `number` | Selector de clientes | Obligatorio; Debe existir en `clientes` | «Elegí un registro» |
| `tipo` | TEXT → `string` | Selector / fichas | Obligatorio; Uno de: `fiado`, `abono` | «Elegí una opción» |
| `monto` | REAL → `number` | Numérico decimal | Obligatorio; Mayor a 0; Hasta 2 decimales | «Ingresá un monto mayor a 0» |
| `nota` | TEXT → `string \| null` | Texto largo | Opcional; Hasta 280 caracteres | «Máximo 280 caracteres» |
| `fecha` | TEXT → `string` | Fecha | Obligatorio; Fecha real en formato AAAA-MM-DD | «Fecha no válida» |
| `anulado` | INTEGER → `number` | Interruptor | Obligatorio; 0 o 1; Valor inicial `0` | — |

**Índices recomendados** (agregarlos en la migración si aún no existen):

```sql
CREATE INDEX IF NOT EXISTS idx_movimientos_cliente_id ON movimientos(cliente_id);
CREATE INDEX IF NOT EXISTS idx_movimientos_fecha ON movimientos(fecha);
```

**Consultas base** (en `src/db/movimientos.ts`):

```ts
listar(): Promise<Movimiento[]>            // SELECT * FROM movimientos ORDER BY id DESC
obtener(id: number): Promise<Movimiento | null>   // SELECT * FROM movimientos WHERE id = ?
crear(datos: NuevoMovimiento): Promise<number>   // INSERT INTO movimientos (cliente_id, tipo, monto, nota, fecha, anulado) VALUES (?, ?, ?, ?, ?, ?)
actualizar(id: number, datos: NuevoMovimiento): Promise<void>   // UPDATE movimientos SET cliente_id = ?, tipo = ?, monto = ?, nota = ?, fecha = ?, anulado = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM movimientos WHERE id = ?
listarPorCliente(cliente_id: number): Promise<Movimiento[]>   // WHERE cliente_id = ?
```

### 3.3 `ajustes`

Configuración clave-valor (nombre de la tienda, plantilla de mensaje).

| Campo | Tipo SQL → TS | Control en formulario | Validación | Mensaje de error |
|---|---|---|---|---|
| `clave` | TEXT | Automático | Clave primaria | — |
| `valor` | TEXT → `string` | Texto | Obligatorio; Texto de 1 a 80 caracteres sin espacios sobrantes | «Escribí valor» |

**Consultas base** (en `src/db/ajustes.ts`):

```ts
listar(): Promise<Ajuste[]>            // SELECT * FROM ajustes ORDER BY id DESC
obtener(id: number): Promise<Ajuste | null>   // SELECT * FROM ajustes WHERE id = ?
crear(datos: NuevoAjuste): Promise<number>   // INSERT INTO ajustes (valor) VALUES (?)
actualizar(id: number, datos: NuevoAjuste): Promise<void>   // UPDATE ajustes SET valor = ? WHERE id = ?
eliminar(id: number): Promise<void>      // DELETE FROM ajustes WHERE id = ?
```

## 4. Tipos TypeScript

Copiar a `src/db/tipos.ts`:

```ts
export interface Cliente {
  id: number;
  nombre: string;
  telefono: string | null;
  limite: number | null;
  activo: number;
}
export type NuevoCliente = Omit<Cliente, 'id'>;

export interface Movimiento {
  id: number;
  cliente_id: number;
  tipo: string;
  monto: number;
  nota: string | null;
  fecha: string;
  anulado: number;
}
export type NuevoMovimiento = Omit<Movimiento, 'id'>;

export interface Ajuste {
  clave: string;
  valor: string;
}
export type NuevoAjuste = Omit<Ajuste, 'id'>;

```

## 5. Valores permitidos (enumeraciones)

SQLite no tiene tipo enumerado: se valida en la capa de datos y se muestra con la etiqueta de la tabla.

**`movimientos.tipo`**

| Valor guardado | Etiqueta en pantalla |
|---|---|
| `fiado` | Fiado |
| `abono` | Abono |

```ts
export const MOVIMIENTOS_TIPO = ['fiado', 'abono'] as const;
```

## 6. Migraciones

Migración 1 = esquema inicial (ya en `src/db/schema.ts` tras la fase 1). Cada cambio posterior es una migración nueva al final de la lista; nunca se edita una ya publicada.

No se prevén migraciones adicionales dentro del MVP.

## 7. Esquema inicial completo

```sql
CREATE TABLE clientes (id INTEGER PRIMARY KEY AUTOINCREMENT, nombre TEXT NOT NULL, telefono TEXT, limite REAL, activo INTEGER NOT NULL DEFAULT 1);
CREATE TABLE movimientos (id INTEGER PRIMARY KEY AUTOINCREMENT, cliente_id INTEGER NOT NULL REFERENCES clientes(id), tipo TEXT NOT NULL, monto REAL NOT NULL, nota TEXT, fecha TEXT NOT NULL, anulado INTEGER NOT NULL DEFAULT 0);
CREATE TABLE ajustes (clave TEXT PRIMARY KEY, valor TEXT NOT NULL);
```

## 8. Reglas de integridad que NO delega a la base

- **RN-01** Un movimiento es un fiado (aumenta la deuda) o un abono (la reduce).
- **RN-02** Saldo del cliente = suma de fiados − suma de abonos, sin contar los movimientos anulados.
- **RN-03** Un abono mayor al saldo se rechaza o se registra como saldo a favor tras confirmación.
- **RN-04** Si el cliente tiene límite, un fiado que lo supere pide confirmación explícita.
- **RN-05** Los movimientos no se borran: se anulan con una marca y se excluyen del saldo.
- **RN-06** La antigüedad de la deuda cuenta desde el fiado pendiente más antiguo (método primero en entrar, primero en pagar).
- **RN-07** El estado de cuenta lista los movimientos no anulados y el saldo final a la fecha.

Estas reglas se implementan en `src/lib/` como funciones puras y se prueban de forma automática (ver [PRUEBAS.md](PRUEBAS.md)).
