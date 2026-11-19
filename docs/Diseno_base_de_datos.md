# Scorer · Diseño de la base de datos · versión 4 (18/11/2026)

Versión 4 · 18/11/2026 · Estado: **implementado en la versión 6.0**. Sustituye a la versión 3.

## Contexto y objetivo

Scorer guarda un directorio fijo de jugadores y equipos para que las estadísticas de varios partidos sumen bien. Antes de la 6.0 cada partido era una ficha cerrada, con los nombres escritos como texto.

**Criterio de identidad (cambia en la v4):** un jugador es un **nombre y un dorsal dentro de su equipo**. Dos jugadores pueden llamarse igual. Lo que no puede haber es dos jugadores del mismo equipo con el mismo dorsal. La app no intenta reconocer a la misma persona en dos equipos: si alguien juega en dos equipos, tiene una ficha en cada uno y sus estadísticas van por separado.

## Decisiones tomadas

| Tema | Decisión | Consecuencia en el diseño |
| --- | --- | --- |
| Quién anota | Cada partido se anota en un solo móvil | No hace falta sincronizar; exportar e importar basta |
| Identidad del jugador | Nombre + dorsal dentro de su equipo (v4) | Cada ficha pertenece a un equipo; el dorsal no se repite dentro del equipo |
| Jugadores en varios equipos | Una ficha por equipo, sin unir entre equipos (v4) | No hay invitados, ni fusiones, ni «Es otra persona» |
| Texto predictivo | Eliminado (v4) | El nombre y el dorsal se escriben; la app enlaza o crea la ficha |
| Temporadas | Todo junto por ahora, pero la base queda preparada para varias temporadas (v4) | Fichas y partidos llevan `temporada`; cada jugador lleva `persona` (vacío) |
| Borrar fichas | Se borran de verdad; los partidos conservan su hoja y sus estadísticas (v4) | Desaparece el «archivado» de la v3 |

## Estructura

Tres cajas que se citan entre sí: **jugadores**, **equipos** y **partidos**. Las estadísticas no se guardan: se calculan leyendo los partidos.

1. Cada **jugador** pertenece a un equipo (`eid`) y figura en su plantilla.
2. Cada **partido** apunta a sus dos equipos y a los jugadores de su line-up, sustitutos y lanzadores.
3. Las **estadísticas** leen los partidos. Nada apunta a ellas.

## Reglas de funcionamiento

- **Código fijo.** Cada jugador, equipo y partido tiene un código interno que no cambia nunca.
- **Foto del día.** Cada fila del line-up y de lanzadores guarda, además del código, el nombre, el dorsal y la posición de ese partido. La hoja antigua se ve igual aunque luego se edite o se elimine la ficha.
- **Sin alta previa obligatoria.** Se anota escribiendo. La pantalla «Jugadores y equipos» permite además dar de alta, editar y eliminar.
- **Enlace al guardar.** Al guardar dorsal y nombre de una fila, la app busca en la plantilla del equipo:
  - mismo dorsal y mismo nombre → enlaza con esa ficha;
  - mismo dorsal y otro nombre → **no deja guardar** y explica quién lleva ese número;
  - sin dorsal → enlaza con el único jugador del equipo con ese nombre, si lo hay;
  - si no existe → crea la ficha en el equipo.
- **Dorsal único en el equipo.** Se comprueba en la hoja (también entre filas del mismo partido), en la ficha de sustitución y en «Jugadores y equipos». Un mismo jugador no puede ocupar dos filas del line-up; sí puede estar en el line-up y en la tabla de lanzadores.
- **Equipo sin nombre.** Si el equipo aún no tiene nombre, las filas se guardan como texto. Al poner el nombre del equipo se enlazan todas; las que choquen con un dorsal ocupado quedan sin enlazar y se avisa.
- **Copiar line-up.** Al elegir un equipo existente, la app ofrece copiar el line-up de su último partido.
- **Sustitutos.** En la hoja suman en la fila del titular, como en papel. En las estadísticas, cada turno cuenta para quien estaba en esa fila en esa casilla exacta (A4).
- **Lanzadores.** Se enlazan con la misma regla, dentro del equipo que defiende: el lanzador es la misma ficha que cuando batea si coinciden dorsal y nombre.
- **Estadísticas calculadas.** Si se corrige una jugada, el informe sale corregido solo.

## Exportar e importar

Se mantiene el archivo de partido (.json) de la v5.0. Exportar e importar el directorio queda para una pieza posterior (botón «Próximamente» en el menú). Con el criterio de la v4, al importar un directorio cada jugador se reconocerá por su código y, si no existe, por equipo + dorsal.

## Preparación para temporadas (pendiente de programar)

- Cada jugador, equipo y partido guarda `temporada` (hoy siempre `"T1"`).
- Cada jugador guarda `persona` (hoy vacío): servirá para enlazar a la misma persona entre temporadas o equipos y sacar estadísticas históricas.
- Cuando se programe, una temporada nueva copiará equipos y plantillas para poder cambiar jugadores y dorsales sin tocar la temporada anterior.

## Anexo técnico

### A1. Las jugadas como datos con significado

Cada casilla lleva un bloque `turno` que la app **recalcula entera al guardar el partido** a partir de lo anotado. Las estadísticas solo leen `turno`. Nunca se edita a mano.

| Campo de `turno` | Contenido |
| --- | --- |
| `fila`, `inning`, `colId` | Dónde está la casilla |
| `bateador`, `corredor` | `{ jid, nombre, dorsal }` de quien bateó y de quien corrió (el corredor emergente, si lo hay) |
| `lanzador` | `{ jid, nombre, dorsal }` del lanzador rival en ese momento, según la tabla y los cambios marcados |
| `resultado` | 1B, 2B, 3B, HR, BB, IBB, HBP, FC, E, K3 (tercer strike caído), K, KC, F, FF, L, P, IF, FO, U, SH, SF, o vacío si no fue aparición al plato |
| `aparicion`, `turnoOficial` | Si cuenta como PA y como AB |
| `baseAlcanzada`, `carrera` | De 0 a 4; carrera = los cuatro tramos recorridos o HR |
| `impulsadas`, `k3`, `lob`, `desempate` | Copia o deducción directa |
| `out` | Número, si fue como corredor, tramo, código, defensores y DP/TP |
| `avances` | Avances como corredor: tramo, causa (SB, E, WP, PB o jugada del bateador), defensor y fila que bateaba |
| `errores` | Posición y jugador de cada error rival en la jugada (WP y PB no son errores) |
| `direccion`, `lanzamientos` | Copia de lo anotado |

### A2. Campos de cada ficha

**Jugador:** `id` ("j" + código), `nombre`, `dorsal` (único en su equipo; puede estar vacío), `pos` (de "1" a "11"; 11 = bateador designado), `eid` (su equipo), `temporada`, `persona`, `creado`.

**Equipo:** `id` ("e" + código), `nombre` (no se repite, sin distinguir mayúsculas ni acentos), `plantilla` (códigos de sus jugadores), `temporada`, `creado`.

**Partido:** `id`, `familia`, `version`, `creado`, `modificado` como en la v5.0, y en `data`:

| Campo | Contenido |
| --- | --- |
| `formato` | 2 |
| `temporada` | "T1" |
| `terminado` | Sí o no; se marca desde ⋯ |
| `partido` | Fecha, hora y estadio |
| `equipos.visitante` / `.local` | `nombre`, `eid`, `filas` (9 o 10), columnas de inning, 10 filas de line-up (se usan 9 o 10), tabla de lanzadores con `jid` y casillas |

**Fila de line-up:** `titular` `{ jid, dorsal, nombre, pos }`; `sustituto` igual más `tipo` (normal o corredor), `desde` (código de la columna de la casilla del cambio) e `inning`; `cambiosPos`.

**Casilla:** como en la v5.0, más: `batter.play` admite `WP` y `PB` y la marca `k3` (tercer strike caído); `runners[i].play` admite `WP` y `PB`; `corredor` (corredor emergente) y el bloque `turno`.

### A3. Fusiones

**Eliminado en la v4.** Con el criterio nombre + dorsal por equipo no se unen fichas.

### A4. Sustituciones por casilla y corredor emergente

- **Sustitución normal:** se marca en la casilla del último turno del que sale. Todo lo de esa casilla es del que sale; el sustituto cuenta desde el siguiente turno de esa fila.
- **Corredor emergente:** botón en la casilla, activo solo si el bateador llegó a base (también con tercer strike caído, WP o PB). En esa casilla el turno al bate es del bateador y lo que pasa como corredor es del corredor; desde el siguiente turno batea el corredor, que ocupa la posición del bateador.
- Cada fila tiene una sola línea de sustituto.

### A5. Borrar jugadores, equipos y partidos

| Qué se borra | Qué pasa |
| --- | --- |
| Jugador | Se pide confirmación. Si ha jugado, se avisa de en cuántos partidos terminados y sin terminar. Desaparece del directorio; sus partidos conservan la hoja y sus estadísticas siguen saliendo |
| Equipo | Se pide confirmación («¿Seguro?»), con el mismo aviso. Se borran el equipo y todos sus jugadores; sus partidos y estadísticas se conservan |
| Partido | Como en la v5.0, con confirmación |

### A6. Versiones de formato y protección

| Qué | Versión |
| --- | --- |
| Base de datos del navegador `anotacion-softball` | 2 (cajas `partidos`, `jugadores`, `equipos`) |
| Partido (`data.formato`) | 2; los partidos sin este campo se convierten al abrirlos |
| Archivo de partidos exportado | 1 |

- La app convierte al abrir cualquier partido antiguo: completa `filas`, `terminado` y `formato`.
- Pide almacenamiento persistente al móvil. Conviene usarla instalada en la pantalla de inicio y exportar de vez en cuando como copia de seguridad.

## Historial de versiones

| Versión | Fecha | Cambios |
| --- | --- | --- |
| 4 | 18/11/2026 | Identidad nombre + dorsal por equipo; sin texto predictivo, invitados ni fusiones; borrar de verdad conservando partidos; preparación para temporadas; WP/PB, tercer strike caído, DH (posición 11), 9 o 10 bateadores y partido terminado |
| 3 | 18/11/2026 | Corredor emergente: posición e inning |
| 2 | 18/11/2026 | Sustituciones por casilla y anexo técnico |
| 1 | 18/11/2026 | Directorio, texto predictivo, exportar e importar |
