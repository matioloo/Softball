# Scorer · Índice del proyecto

Versión 2 · 18/11/2026 · Estado: **versión 6.0 terminada y lista para subir a GitHub.** Sustituye a la versión 1.

## 0. Para qué sirve este documento

Es el punto de entrada para cualquier conversación que vaya a modificar el código: qué archivos hay, qué papel tiene cada uno, quién manda cuando no coinciden y qué queda pendiente.

**Regla de oro:** este índice manda sobre los demás documentos.

**Cómo empieza DEV cualquier conversación de construcción:**

1. Leer este índice.
2. Leer solo los documentos que necesita el cambio.
3. Partir del código publicado en GitHub (`https://raw.githubusercontent.com/matioloo/Softball/main/index.html`), nunca de memoria. La API de GitHub limita las consultas; las direcciones directas de archivo funcionan.
4. Leer los archivos grandes por trozos, con herramientas.
5. Probar los cambios como artefacto (con datos de prueba que no se publican) y publicar en GitHub solo al final.

## 1. Archivos y su papel

| Archivo | Versión | Qué es | Estado |
|---|---|---|---|
| `Indice_proyecto_Scorer.md` | 2 | Este índice | Vigente. Manda sobre todo |
| Repositorio GitHub `matioloo/Softball` (`main`) | 6.0 | Código en producción: `index.html`, `sw.js`, `manifest.webmanifest`, `icons/`, `fonts/`, `docs/` | Vigente. Fuente de verdad del código |
| `Diseno_base_de_datos.md` | 4 | Datos: fichas, partidos, jugadas como datos (`turno`), sustituciones, borrado, versiones, preparación para temporadas | Vigente |
| `Especificacion_directorio_jugadores_y_equipos.md` | 4 | Directorio: identidad nombre + dorsal por equipo, hoja, edición y borrado | Vigente |
| `Especificacion_informes_de_estadisticas.md` | 2 | Informes de jugador y de equipo: pantalla con zoom, cálculos y reglas | Vigente |
| `Referencia_tarjetas_estadisticas.html` | Aprobada | Maqueta visual de las tarjetas | Vigente para medidas y estilos |
| `Logo_Scorer.png` | — | Muestra del logo; el logo de la app está redibujado en vector | Referencia |
| `Diseño_interfaz.md` | Encargo original | Primer encargo de la hoja | **Histórico** |
| `Plantilla_de_béisbol_visitante_y_local.png` | — | Hoja en papel que inspiró el diseño | **Histórico** |

`Referencia_directorio_prototipo.html` ya no existe en el proyecto: su contenido está en el código de la 6.0.

## 2. Quién manda

1. Este índice.
2. **Qué se guarda y cómo:** `Diseno_base_de_datos.md` v4.
3. **Comportamiento de la app:** el código publicado en GitHub.
4. **Directorio:** `Especificacion_directorio_jugadores_y_equipos.md` v4.
5. **Informes:** `Especificacion_informes_de_estadisticas.md` v2 y, para el aspecto, la referencia de tarjetas.
6. Los documentos históricos no mandan nunca.

## 3. Qué incluye la 6.0

- **Nombre y logo Scorer** en la pantalla de inicio, la hoja, la casilla, las tarjetas y los iconos de instalación (iPhone con fondo color tierra batida entero).
- **Directorio de jugadores y equipos** con el criterio nombre + dorsal por equipo, dorsal único dentro del equipo, edición y borrado con avisos, y copiar line-up. Sin texto predictivo.
- **Hoja:** 9 bateadores ampliable a 10; la posición 10 solo en los menús defensivos si alguien la ocupa; bateador designado como posición 11 (DH); WP y PB en el menú E#, guardados aparte de los errores; tercer strike caído («K WP», «K PB», «K E3»); sustituciones por casilla y corredor emergente; «Marcar partido como terminado».
- **Dirección del batazo:** se toca donde bota la bola (o donde la atrapan), con franja HR detrás de la valla.
- **Estadísticas:** informes de jugador y de equipo en pantalla, con zoom como la hoja.
- **Base de datos preparada para temporadas** (campos `temporada` y `persona`).

## 4. Decisiones del usuario que fijan la 6.0

| Tema | Decisión |
|---|---|
| Identidad del jugador | Nombre + dorsal dentro del equipo; sin unir personas entre equipos |
| Dorsal repetido en un equipo | Se avisa y no se deja guardar, en cualquier panel |
| Texto predictivo, invitados, revisión de jugadores nuevos | Eliminados |
| Eliminar jugador o equipo que ha jugado | Se avisa (partidos terminados y sin terminar); sus hojas y estadísticas se conservan |
| WP y PB | Se anotan desde E# y se guardan como WP y PB |
| Tercer strike caído | Desde E# del bateador; cuenta como AB y K |
| Partido terminado | Lo marca el usuario desde ⋯ |
| Partidos sin terminar | Fuera del balance, resultados y carreras por entrada; dentro del resto |
| Mínimo de AB para el líder de AVG | 2,5 por partido del equipo |
| Color del mapa | Rosa en tarjetas, verde en la hoja |
| Informe | En pantalla con zoom; PDF pendiente |

## 5. Pendiente para próximas versiones

1. **Exportar e importar jugadores y equipos** entre móviles (botón «Próximamente»).
2. **Temporadas:** crear temporadas, cambios de jugadores y equipos entre ellas y estadísticas históricas, usando `temporada` y `persona`.
3. **PDF de las tarjetas** de estadísticas.
4. Informes interactivos más ricos.
5. Segundo cambio en la misma fila del line-up.
6. Automatizar las carreras de los corredores en base al marcar HR.

## 6. Cómo se publica una versión en GitHub

1. Subir `index.html`, `sw.js` y `manifest.webmanifest` a la raíz, y los iconos a `icons/`, reemplazando los anteriores.
2. Cambiar `VERSION` en `sw.js` en cada publicación (ahora `scorer-6.0`), para que los móviles instalados se actualicen solos.
3. Guardar los documentos vigentes en `docs/`.
4. Probar en el móvil: abrir la app con conexión, cerrarla y volver a abrirla para que cargue la versión nueva.

## Historial

| Versión | Fecha | Cambios |
|---|---|---|
| 2 | 18/11/2026 | 6.0 terminada: nuevo criterio de jugador, directorio editable, informe en pantalla con zoom, PDF pendiente, archivos vigentes y pendientes actualizados |
| 1 | 18/11/2026 | Primera versión: alcance y plan de la 6.0 |
