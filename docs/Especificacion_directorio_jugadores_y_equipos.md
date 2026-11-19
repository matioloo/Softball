# Scorer · Especificación del directorio de jugadores y equipos

Versión 4 · 18/11/2026 · Estado: **implementado en la versión 6.0**. Sustituye a la versión 3.

Se lee junto con `Diseno_base_de_datos.md` (versión 4), que fija los datos, y con `Indice_proyecto_Scorer.md`.

**Qué cambia respecto a la versión 3:** un jugador es un nombre y un dorsal dentro de su equipo; desaparecen el texto predictivo, los invitados de otro equipo, «Es otra persona», el aviso «¿Querías decir…?» y la revisión «Jugadores nuevos en este partido»; se pueden editar y eliminar jugadores y equipos.

---

## 1. Principios

- **Se anota escribiendo.** Nunca es obligatorio dar de alta antes de anotar.
- **Jugador = nombre + dorsal en su equipo.** Dos jugadores pueden llamarse igual; dos jugadores del mismo equipo no pueden tener el mismo dorsal.
- **Foto del día.** Cada fila del partido guarda el código de la ficha y además el nombre, dorsal y posición de ese día.
- **Nunca se borra nada sin confirmación,** y borrar una ficha no borra partidos ni estadísticas.

## 2. En la hoja

### 2.1 Equipo

Al tocar el nombre del equipo se abre un panel con el nombre y la lista de **equipos guardados** para elegir uno con un toque. Al guardar, se enlaza con el equipo de nombre idéntico (sin distinguir mayúsculas ni acentos) o se crea uno nuevo. Después se enlazan todas las filas ya escritas de ese lado; las que choquen con un dorsal ocupado quedan sin enlazar y se avisa. Si el line-up está vacío y el equipo tiene partidos anteriores, se ofrece **copiar el line-up** del último.

### 2.2 Jugador (titular, sustituto o lanzador)

Al tocar el dorsal o el nombre de una fila se abre un único panel con **dorsal y nombre**. Al pulsar Guardar:

| Situación | Resultado |
|---|---|
| Nombre vacío | Se vacía la fila (con dorsal escrito, pide el nombre) |
| Otra fila de este partido y equipo tiene ese dorsal con otro nombre | **No se guarda.** «El #12 ya lo lleva Juan Pérez en este partido…» |
| El mismo jugador ya está en otra fila del line-up | **No se guarda.** «… ya está en el line-up, en el n.º bate» (en la tabla de lanzadores sí puede repetirse) |
| En la plantilla, ese dorsal es de otro nombre | **No se guarda.** «En Tigres, el #12 es Juan Pérez. Cambia el número o, si ha cambiado de dorsal, corrígelo antes en Jugadores y equipos.» |
| Mismo dorsal y nombre en la plantilla | Se enlaza; si la fila no tenía posición, toma la habitual |
| Sin dorsal y un único jugador del equipo con ese nombre | Se enlaza y se rellena su dorsal |
| Corrección de una ficha creada en esta misma fila y no usada en ningún otro sitio | Se corrige la ficha en vez de crear otra |
| Jugador nuevo | Se crea la ficha en el equipo: «Jugador nuevo guardado en Tigres: …» |
| Equipo sin nombre todavía | La fila se guarda como texto; se enlaza al poner el equipo |

El error aparece en rojo dentro del panel y el panel no se cierra hasta corregirlo.

### 2.3 Ficha de sustitución y corredor emergente

Igual que en la versión 3 (casilla exacta, posición e inning propuestos), con las mismas comprobaciones de dorsal del apartado 2.2.

## 3. Pantalla «Jugadores y equipos»

- Un bloque por equipo, en orden alfabético, con el número de jugadores; dentro, los jugadores por dorsal, con su posición habitual.
- **Tocar el nombre del equipo:** editar su nombre (no puede repetirse) o **«Eliminar equipo»**. Pide confirmación: «¿Eliminar Tigres y sus 11 jugadores?», con el aviso de partidos jugados («Tigres ha jugado en 2 partidos terminados y 1 partido sin terminar…») y «¿Seguro?». Se borran el equipo y sus jugadores; partidos y estadísticas se conservan.
- **Tocar un jugador:** editar dorsal, nombre y posición habitual, o **«Eliminar»**. El dorsal no puede repetirse en su equipo. Al eliminar se avisa de los partidos jugados, distinguiendo terminados y sin terminar, o «No ha jugado ningún partido». «Cancelar» vuelve a la ficha.
- **Nuevo equipo** y **Nuevo jugador** (con dorsal, nombre, posición habitual y equipo obligatorio).
- Corregir el nombre o el dorsal de una ficha no cambia las hojas ya anotadas (foto del día), pero los informes muestran el nombre corregido.

## 4. Textos de interfaz

| Lugar | Texto |
|---|---|
| Panel de jugador sin equipo | «Escribe antes el nombre del equipo para que el jugador se guarde en Jugadores y equipos.» |
| Dorsal repetido (directorio) | «En Halcones el #11 ya es de Javier Herrera. Dos jugadores del mismo equipo no pueden tener el mismo número.» |
| Aviso al eliminar | «Juan Pérez ha jugado en 2 partidos terminados y 1 partido sin terminar. … Las hojas y las estadísticas de esos partidos se conservan.» |
| Avisos | «Jugador nuevo guardado en …», «Equipo nuevo guardado: …», «Line-up copiado.», «Jugador eliminado: …», «Equipo eliminado: …» |

## 5. Pruebas de aceptación (QA, superadas en el prototipo)

1. Dorsal repetido en el directorio: no deja guardar.
2. Editar nombre y dorsal de un jugador.
3. Eliminar a quien no ha jugado: aviso «No ha jugado ningún partido»; cancelar vuelve a la ficha.
4. Eliminar a quien ha jugado: aviso con partidos terminados y sin terminar; sus estadísticas siguen saliendo.
5. Eliminar un equipo con sus jugadores, con confirmación.
6. En la hoja, un dorsal del equipo con otro nombre: no deja guardar.
7. En la hoja, el mismo jugador en dos filas del line-up: no deja guardar.
8. En la hoja, #12 Juan Pérez se enlaza con su ficha; un jugador nuevo se crea en el equipo.
9. Partidos de la 5.0: se abren con 9 filas (o 10 si la décima tenía datos) y se pueden anotar.

## 6. Pendiente

- Exportar e importar el directorio entre móviles.
- Temporadas: ver «Preparación para temporadas» en el diseño de base de datos.
