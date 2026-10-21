# CLAUDE.md — Contramuestras (contramuestras)

Las reglas generales de trabajo están en el repo privado `lives-instrucciones` (CLAUDE.md). Este archivo solo describe esta herramienta.

## Qué hace
Almacén de contramuestras de Desarrollo. Una contramuestra es la prenda de referencia que se guarda del desarrollo de un estilo, archivada en una caja numerada (taper), con su fase (proto, SMS, fit sample…). Estados: DISPONIBLE, PRESTADA, DEPURADA.
- `index.html` — **almacén**: consulta el inventario y registra movimientos (INGRESAR, PRESTAR, REGISTRAR DEVOLUCIÓN, DEPURAR). Ventanas de Solicitudes (bandeja), Editar prenda (⚙), Depuración (prendas que pasaron el plazo), Gráficos (por taper y por cliente) y Listas/configuración. La bandeja se revisa cada minuto.
- `buscar.html` — **consulta para analistas**: buscan una prenda y, si está DISPONIBLE, envían una solicitud al almacén con motivo. Las solicitudes llegan a la bandeja de `index.html`.
- Las escrituras no se reintentan solas, para no duplicar ingresos.

## Base de datos (Supabase lives-datos)
- Leer: `cm_inicio` (listas, inventario, solicitudes, configuración y permisos de la persona) y `cm_pendientes` (bandeja).
- Registrar: `cm_ingresar`, `cm_prestar`, `cm_devolver`, `cm_depurar`, `cm_editar` (tabla `cm_prendas`); `cm_atender` (`cm_solicitudes`); `cm_guardar_listas` (`cm_listas`, `cm_config`); el historial queda en `cm_movimientos`.
- Solicitar (`buscar.html`): `cm_solicitar` (`cm_solicitudes`).
- No usa Storage ni lee tablas directo.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 5 personas de Desarrollo (analistas, asistente y responsable de registro).

## Quién puede registrar
- Lo decide la base: `cm_puede_registrar_` (almacén) y `cm_puede_solicitar_` (pedir prendas), con la tabla `permisos`. Sin permiso la pantalla queda en "Solo consulta".
- A Sistemas: esas dos funciones son las piezas que reemplazan con su seguridad.
