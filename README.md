<div align="center">

<img src="docs/assets/logo.svg" alt="CuentaClara" width="96" height="96"/>

# CuentaClara

**Libreta digital de fiados para tiendas de barrio: quién debe, cuánto y desde cuándo, con estados de cuenta en PDF. Sin internet.**

<p>
<img src="https://img.shields.io/badge/Expo-000020?style=for-the-badge&logo=expo&logoColor=white" alt="Expo"/>
<img src="https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB" alt="React Native"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript"/>
<img src="https://img.shields.io/badge/SQLite-003B57?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite"/>
<img src="https://img.shields.io/badge/Licencia-MIT-green?style=for-the-badge" alt="MIT"/>
</p>

[🚀 Características](#-características) · [🧰 Stack](#-stack) · [📂 Estructura](#-estructura) · [🔧 Configuración](#-configuración) · [🧪 Pruebas](#-pruebas) · [🤝 Contribuir](#-contribuir) · [📄 Licencia](#-licencia)

</div>

---

## 🚀 Características

- **Clientes y saldos**: deuda total y antigüedad por cliente.
- **Fiados y abonos**: registra ventas a crédito y pagos parciales.
- **Estado de cuenta en PDF** para compartir.
- **Recordatorio de cobro** por WhatsApp pre-llenado.
- **Resumen**: total por cobrar y mayores deudores.
- **Respaldo local** exportable.

> Sin servidores, sin suscripciones y sin servicios de pago: todos los datos viven en el dispositivo (SQLite).

## 🧰 Stack

| Capa | Tecnología |
| --- | --- |
| App | React Native + Expo (TypeScript) |
| Datos | SQLite local (`expo-sqlite`) |
| Navegación | Expo Router |
| Distribución | APK / EAS Build local |

### Modelo de datos

- `clientes (id, nombre, telefono, limite)`
- `movimientos (id, cliente_id, tipo, monto, nota, fecha)`
- `ajustes (clave, valor)`

## 🗺️ Roadmap

- [ ] 1. Estructura base y navegación
- [ ] 2. Clientes y movimientos
- [ ] 3. Estados de cuenta en PDF
- [ ] 4. Recordatorios por WhatsApp
- [ ] 5. Respaldo y restauración

> Detalle fase por fase: [docs/ROADMAP.md](docs/ROADMAP.md) · [docs/SPEC.md](docs/SPEC.md) · [docs/PLANNING.md](docs/PLANNING.md) · [docs/DATOS.md](docs/DATOS.md) · [docs/PANTALLAS.md](docs/PANTALLAS.md) · [docs/PRUEBAS.md](docs/PRUEBAS.md) · [docs/BACKLOG.md](docs/BACKLOG.md)

## 📂 Estructura

```text
CuentaClara/
├── app/            # Pantallas y rutas (Expo Router)
├── src/
│   ├── db/         # Esquema y consultas SQLite
│   ├── components/ # Componentes reutilizables
│   └── lib/        # Utilidades
├── docs/assets/    # Logo y recursos del README
└── .github/        # Plantillas, CI y Dependabot
```

## 🔧 Configuración

Requisitos: Node.js 20+ y la app Expo Go (o un emulador).

```bash
git clone https://github.com/Luiss2080/CuentaClara.git
cd CuentaClara
npm install
npx expo start
```

> El repositorio está en fase inicial: la estructura y la documentación están listas y el código de la app se construye siguiendo el roadmap.

## 🧪 Pruebas

```bash
npm test
npx tsc --noEmit
```

## 🤝 Contribuir

Lee la [guía de contribución](CONTRIBUTING.md) y el [código de conducta](CODE_OF_CONDUCT.md). Para reportar vulnerabilidades consulta la [política de seguridad](SECURITY.md); para dudas, [SUPPORT.md](SUPPORT.md).

## 📄 Licencia

Distribuido bajo licencia MIT. Consulta [LICENSE](LICENSE).

---

<div align="center">

Hecho por [Luiss2080](https://github.com/Luiss2080)

</div>
