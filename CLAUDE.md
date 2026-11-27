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
- Selección desde la base (desde el 11/11/2026): en Ingresar y Editar prenda, si el número del estilo ya tiene OP (está en `ops`), el color se elige de la lista (`ops` + `partidas` + lo ya registrado); un proto sin OP se puede escribir. Lectura `cm_catalogo_base`; `cm_ingresar` y `cm_editar` lo validan con `cm_valida_base_` (al editar, solo si el color cambia).
- Listas cerradas (fase 3 de Desarrollo, 27/11/2026): las fases salen de la maestra `wm_fases` (`en_contramuestras`, activas) y los analistas de `des_personal` (activos con rol ANALISTA; altas por la pantalla Catálogos); ya no se escriben en la ventana Listas, que solo deja Clientes, motivos y configuración. `cm_ingresar`, `cm_editar` y `cm_prestar` rechazan un cliente, una fase o una persona fuera de lista con `cm_valida_listas_` (al editar, cliente y fase solo si cambian). Las filas FASES y ANALISTAS de `cm_listas` quedan sin uso. Desde el mismo día (decisiones de Christian): en la lista de personas salen también las ejecutivas (roles ANALISTA o EJECUTIVA), y los clientes son los mismos del WIP de Muestras (`des_clientes_muestras_`: `wm_clientes` + `cat_clientes`), ya no `cm_listas`; en el historial PERUVIAN pasó a PERUVIAN CONNECTION y MM LAFLEUR a M.M. LAFLEUR (mismos códigos PEC y MMF; respaldo `respaldo_ds05_cm_clientes`). La ventana Listas solo deja los dos motivos y la configuración. Un cliente nuevo se agrega en la maestra de clientes, no aquí. Clientes solo de Contramuestras: la lista `CLIENTES_EXTRA` de `cm_listas` se suma al final de los clientes del WIP (hoy solo NO IDENTIFICADO, que no tiene código de cliente; `cm_listas_` la une y `cm_valida_listas_` la acepta; no se edita desde la pantalla y el WIP no la ve). El estilo se sigue escribiendo (los protos no tienen OP): va anotado para el paquete de Sistemas.
- No usa Storage ni lee tablas directo.

## Quién la usa
Según las entradas por el portal de los últimos 30 días al 21/10/2026: 5 personas de Desarrollo (analistas, asistente y responsable de registro).

## Quién puede registrar
- Lo decide la base: `cm_puede_registrar_` (almacén) y `cm_puede_solicitar_` (pedir prendas), con la tabla `permisos`. Sin permiso la pantalla queda en "Solo consulta".
- A Sistemas: esas dos funciones son las piezas que reemplazan con su seguridad.
