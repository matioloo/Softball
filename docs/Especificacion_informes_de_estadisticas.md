# Scorer · Especificación de los informes de estadísticas

Versión 2 · 18/11/2026 · Estado: **informe en pantalla implementado en la versión 6.0**. El PDF queda pendiente. Sustituye a la versión 1.

Se lee junto con `Referencia_tarjetas_estadisticas.html` (maqueta aprobada: medidas, estilos y orden de bloques), `Diseno_base_de_datos.md` (versión 4) e `Indice_proyecto_Scorer.md`.

**Qué cambia respecto a la versión 1:** el informe se abre en pantalla, sobre la app, con zoom como la hoja; el PDF pasa a pendiente; se cierran los puntos a confirmar; se fijan el reparto de PO en el doble play y las reglas de partidos sin terminar; con el criterio nombre + dorsal por equipo, el informe de jugador es el de su ficha en ese equipo.

---

## 1. Alcance y pantalla

- Botón **Estadísticas** del menú principal → pantalla con dos pestañas, **Jugadores** y **Equipos**, ordenadas por nombre. Cada fila dice dorsal, equipo y número de partidos (o balance G–P en los equipos).
- Al tocar uno, su tarjeta se abre **encima de la app**. Se amplía **pellizcando**, se mueve **arrastrando**, en el ordenador el doble clic amplía o vuelve al ajuste, y **«Ajustar»** la encaja en el ancho de la pantalla, igual que la hoja. «Volver» regresa a la lista.
- La tarjeta tiene siempre fondo blanco, también en modo oscuro.
- Solo cuenta la copia más reciente de cada partido (v2, v3…) y los partidos con algo anotado.
- Los informes solo leen datos.

## 2. Contenido de las tarjetas

Como en la versión 1 y en la referencia visual, con estas precisiones:

- **Jugador:** su ficha es nombre + dorsal en un equipo. Quien juega en dos equipos tiene dos fichas y dos informes. Un jugador eliminado del directorio sigue teniendo informe con el nombre de sus hojas.
- **Pitcheo:** el bloque solo aparece si ha lanzado; la nota «solo si ha lanzado» no se muestra.
- **Equipo · Líderes:** AVG exige **2,5 AB por partido del equipo, redondeado**. Empates: los dos nombres, o «Empate (n)» si son más.
- **Equipo · Plantilla:** se resalta el líder en PA, AB, H, HR, R, RBI, SB y AVG (con el mínimo de AB). No se resaltan G, OBP ni E. Orden: orden de bateo más habitual; a igualdad, PA.
- **Color del mapa:** rosa en las tarjetas, verde en la hoja, como estaba aprobado.

## 3. Cálculos (cambios y precisiones)

| Estadística | Regla |
|---|---|
| R | Carrera = los cuatro tramos del rombo recorridos o HR (no existe la marca RUN) |
| RBI | Valor RBI de la casilla, de 0 a 4 |
| K | K, K mirando y tercer strike caído (que además es AB) |
| PO del ponche | Para el receptor, salvo tercer strike caído con llegada a base |
| PO del doble play | En un fielded out con DP o TP de tres o más defensores, PO para el último y para el que recibe el primer pase (en 6-4-3, el 4 y el 3); asistencias para todos menos el último, sin repetir |
| DP | Cada jugada marcada DP o TP suma 1 al equipo y 1 a cada defensor que intervino |
| E | E# del bateador y de los corredores; **WP y PB no son errores** |
| Posiciones | Partidos en que el jugador ocupó cada posición; DH = posición 11; un lanzador que no batea cuenta como P |
| Pitcheo | Desde la tabla de lanzadores de cada partido; ER sigue siendo manual |

**Partidos sin terminar:** el balance G–P, las carreras a favor y en contra de la cabecera, la tabla de resultados y las carreras por entrada usan solo los partidos **marcados como terminados** (⋯ → «Marcar partido como terminado»). Todo lo demás cuenta todos los partidos. La cabecera del equipo dice cuántos están sin terminar.

## 4. PDF (pendiente)

En la 6.0 no hay botón «Exportar PDF». Cuando se haga, valen las opciones A y B de la versión 1: tarjeta de 180 mm de ancho en una sola pieza, probando primero un tamaño de página propio y, si Safari lo corta, generar el PDF dentro de la app.

## 5. Pendiente

- PDF de las tarjetas.
- Versión interactiva más rica (filtros, gráficos).
- Estadísticas por temporada y históricas, cuando se programen las temporadas.
