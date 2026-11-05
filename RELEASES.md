# Releases — instalador del puente de balanza (Erp Los Hermanos)

Distribución pública del instalador del puente de balanza de Erp Los Hermanos (binarios; el
código fuente del puente vive en el repo privado `erp-los-hermanos`, carpeta `app/bridge`).
Reemplaza al repo `erp-los-hermanos-releases`.

Los instaladores están adjuntos a los **Releases** de este repo.

**Descargar siempre la última versión** (link fijo, sirve para el botón del ERP,
`VITE_BALANZA_PUENTE_INSTALADOR_URL`):

https://github.com/matiaspautasso/drsystel-balanza-conexion/releases/latest/download/PuenteBalanza-Setup.exe

> Al publicar una versión nueva, adjuntar el setup **dos veces**: con su nombre versionado
> (`PuenteBalanza-Setup-X.Y.Z.exe`) y como `PuenteBalanza-Setup.exe`, y marcar el release como
> *Latest*. Así el link fijo de arriba pasa a servir la versión nueva sin tocar Railway (solo
> hay que subir `VITE_BALANZA_PUENTE_VERSION_MIN` si se quiere forzar el aviso de actualizar).

| Tag | Fecha | Instalador | SHA256 |
|---|---|---|---|
| `bridge-v1.3.3` | 2026-11-05 | `PuenteBalanza-Setup-1.3.3.exe` | `7257f38f2c32d7204700c12f9655b5d2b447941b25ef615f78fd354cf41350e5` |
| `bridge-v1.3.2` | 2026-11-05 | `PuenteBalanza-Setup-1.3.2.exe` | `37b8c6d89442f7db4ac199b4ca27cdefb4d6faaae63af4f478e4c4bd0262b442` |
| `bridge-v1.3.1` | 2026-11-05 | `PuenteBalanza-Setup-1.3.1.exe` | `dabb3b0bc38e20f6d97ff17696b3ff21760436c7e94f057a7a1b633934df47ad` |
| `bridge-v1.3.0` | 2026-11-05 | `PuenteBalanza-Setup-1.3.0.exe` | `cdbefc0682e2a93903aca7967bcd5c54d35a6ad63d53359d714619a24bb5d55f` |
| `bridge-v1.1.0` | 2026-10-14 | `PuenteBalanza-Setup-1.1.0.exe` | `08ed13670c937a8b012a4c07e071805aaf157c2caf3d151c646531d9566a8775` |
| `bridge-v1.0.0` | 2026-09-25 | `PuenteBalanza-Setup-1.0.0.exe` | `c45812d4162f8df9ff7d2b7df4f146b3d2dffbc85faafb3ed221552714aecbef` |

## 1.3.3

Mínima carga sobre la balanza: la lectura de tickets (F72) se hace una sola vez después de cada
venta (pulso F28 de ~20 B para detectar actividad), sin lectura de respaldo, y solo del día
actual. Antes eran ~7 lecturas completas por venta más una cada ~16 s, con un rango que crecía.

## 1.3.2

Solo se cargan en el ERP los tickets emitidos con la balanza conectada al sistema: en cada
(re)conexión lo que ya tiene la balanza se toma como visto sin enviarse. Las pendientes con
conexión se siguen entregando.

## 1.3.1

Fix de F31 (`GET /plus`): la trama ya no se corta ante la pausa de ~250 ms de la balanza (el
silencio solo cierra la trama con XOR valido). Verificado en HW: 425 PLU en ~0,7 s.

## 1.3.0

Push-on-change con shadow SQLite (`data\shadow-plu.db`): los cambios de PLU se aplican de a uno
(F33/F4) en vez de reconciliar ~400 PLU cada 5 min. El instalador suma `better_sqlite3.node`.
Actualizar encima conserva `config.env` y `data\`. Corregido en 1.3.1: `GET /plus` (F31) fallaba con
XOR incorrecto (trama cortada por silencio de 80 ms).

## 1.1.0

Autodetección del puerto COM de la balanza (adaptador FTDI, VID 0403 / PID 6001):
`BALANZA_COM=auto` por defecto; con un COM fijo que no abre, usa el FTDI detectado. Actualizar
encima conserva `config.env` y `data\`.

## 1.0.0

Primera versión del instalador (Inno Setup, Windows x64): `puente-balanza.exe` + inicio
automático con la tarea programada `ErpLosHermanos-PuenteBalanza`.
