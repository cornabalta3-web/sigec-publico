# Modelos

Buscador de modelos: cuál es el modelo que va para cada trámite, y cuál es el vigente.

- **Estado:** prototipo. `modelos.html` ya se abre desde el menú, con **veinte modelos
  inventados**: el catálogo de verdad se arma leyendo la carpeta del disco R.
- **Archivo:** `modelos.html`
- **En SIGEC:** `id:"modelos"`, grupo Más trámites, color `#55677E` / `#9BAEC4`.

## Qué problema resuelve, que no es el que parece

Hoy se trabaja *a partir de* un modelo: se toma uno y desde ahí se arma el documento.
Están todos cargados en el disco R. **El problema no es llenarlos: es encontrarlos.** Hay
unos veinte modelos distintos y no se sabe cuál es el que va. No pasa en los trámites
comunes —escrituras sociales, DDJJ, testimonios—, que ya tienen herramienta propia. Pasa
en los raros: una autorización a salir del país, un acta de constatación.

Eso cambia el producto por completo:

- **No hay que convertir ningún `.docx` a nada**, ni parsearlos, ni llenarlos. El límite
  de `file://` deja de aplicar: una página puede listar modelos y abrir cada uno en Word
  con un enlace relativo.
- Lo que hace falta es un **catálogo**: para qué sirve cada modelo, cuál es el vigente
  cuando hay varios parecidos, y quién lo dijo.
- **Los veinte son trámites distintos, no versiones del mismo.** No hay problema de
  versionado sino de identificación: es una búsqueda, no un historial.

## El riesgo que hay que resolver con el producto

- Cualquiera puede editar un modelo del disco R: no hay permisos.
- La única señal de cuál vale es la fecha de modificación.

Juntas dan un modo de falla que no necesita que nadie haga nada raro: alguien abre el
modelo, lo adapta a su caso, guarda por costumbre, y el maestro pasó a ser el caso de esa
persona. Como la señal de autoridad es la fecha, **ese accidente queda con la fecha más
nueva y parece el más autorizado de todos.** La heurística premia el error.

Marcar el vigente como DEFINITIVO no alcanza: hay que poner los modelos en solo lectura y
que trabajar sea siempre sobre una copia.

## Lo que hace el prototipo

Un campo de búsqueda y una lista agrupada por trámite. Dos detalles que son casi toda la
herramienta:

- **Cada modelo lleva un campo `busca`** con las palabras que alguien usaría de verdad, que
  casi nunca son las del título. Nadie escribe "venia supletoria": escribe "permiso para
  salir del país". Ahí está la mitad del valor.
- **El que no va se muestra igual, tachado y con un aviso** de cuál lo reemplaza. Esconder
  el viejo no sirve: el que lo busca lo va a encontrar en la carpeta de todos modos, así
  que mejor que la herramienta le diga que ese no es.

El panel de cada modelo repite la regla de la decisión 14: **se abre una copia, no el
maestro**, porque el modo de falla es guardar por costumbre sobre el modelo bueno, y como
la única señal de autoridad es la fecha, ese accidente queda pareciendo el más nuevo.

## Prioridad

Es la 3 de diciembre: fácil, pero no tan importante como pulir lo que ya se usa y como
poner operativas las escrituras sociales.

## Hecho el 15/09/2026

- **El único botón no abría nada.** «Abrir en Word» cambiaba su propio texto a «En el
  prototipo no abre nada» y se restauraba, mientras el párrafo pegado al botón explicaba en
  detalle cómo se abre la copia y cómo queda el maestro en solo lectura. Ahora el botón
  aparece solo cuando el modelo tiene `ruta`, y mientras no la tenga la pantalla dice que
  el catálogo es una muestra y que ninguno está enlazado todavía.
- **El catálogo decía quién había confirmado cada modelo, y eran personas reales.**
  Diecinueve confirmaciones inventadas atribuidas por nombre y apellido a las dos
  escribanas. Ahora dice «Escribanía General».
