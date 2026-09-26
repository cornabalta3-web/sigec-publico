# Primer testimonio

Convierte la escritura de protocolo en primer testimonio, con folio, cierre y
concordancia.

- **Estado:** en uso.
- **Archivo:** `primer-testimonio.html`
- **En SIGEC:** `id:"testimonio"`, grupo Escrituración, color `#3E9480` / `#5FB8A1` en
  oscuro. **El verde no es decorativo:** es el de la hoja de actuación notarial. Por el
  color el usuario se da cuenta de dónde está. No cambiarlo para "unificar".

## Cómo guarda

- `localStorage` — `sigec_escribanas`: los nombres de las escribanas, compartidos con el
  rogador y con expropiaciones. Lo que estaba guardado bajo la clave vieja
  `testimonio_escribanas` se migra solo al abrir, así que nadie vuelve a tipear lo que ya
  tipeó.

  Arrastraba un defecto viejo: usaba `window.storage`, una API que no existe en ningún
  navegador y que nadie define en el proyecto. Guardar y leer fallaban siempre, la
  lectura en un `catch` vacío, mientras el cartel decía "Se guardan para las próximas
  veces". Arreglado y verificado abriendo dos sesiones: la segunda recuerda lo que
  guardó la primera.

## Los nombres de las escribanas son un dato, no código

El cargo lo pone el código —**Escribana General de Gobierno** y **Escribana Sustituta**—
porque es el que nombra la ley y no cambia. El **nombre** de quien ocupa el cargo no está
escrito en ninguna parte del archivo: se tipea una vez y queda guardado bajo
`sigec_escribanas`, una clave que comparten primer testimonio, el rogador y expropiaciones.
Las páginas abiertas con `file://` comparten el `localStorage` del navegador, así que
cargarlo en una herramienta lo carga en las tres. **Verificado en Chromium**: se tipeó en
expropiaciones y primer testimonio lo leyó.

Dos cosas que esto obliga:

- **Si no hay nombre cargado, se dice.** Un documento notarial sin la escribana que lo
  firma no sirve, así que no se deja salir callado.
- **El renglón corto** de los formularios del Registro lleva `Esc. APELLIDO`. Se saca de
  las palabras en mayúscula del nombre cargado, y si no hay ninguna, de la última palabra.
  Lo que va a salir se muestra debajo del campo: no se adivina a escondidas.

Acá la tarjeta **«Las escribanas»** se abre sola cuando no hay ningún nombre cargado: es lo
primero que hay que completar. Y el selector de «Ante mí» dice «sin nombre cargado» en vez
de mostrar un paréntesis vacío.

## No llama a la API

## Pendiente

- Sigue el diseño viejo, con la plantilla escrita en el código. Puede quedar así: la
  migración a plantilla-como-dato no lo tiene como prioridad.

## Hecho el 15/09/2026

- **El campo «Quiénes firman» se perdía en silencio.** El bloque calculaba `let ok=false` y
  después nunca lo leía: si la escritura no contenía literalmente «firman los
  comparecientes», «firman los otorgantes» o «firma el compareciente», el dato no se
  insertaba y el cartel igual decía «Listo». Ahora lo dice, nombra el texto que no pudo
  poner y pide agregarlo en Word.
- **La detección de comparecientes exigía que el apellido viniera en MAYÚSCULAS.** Con
  «Mariana Beatriz Ayala» devolvía cero, sin avisar. `tipoNombre()` reconoce ahora las dos
  formas —devuelve `caps`, `titulo` o `null`— y cuando tuvo que guiarse por la
  capitalización deja una nota visible pidiendo que se revisen. Es la regla general del
  proyecto: la mayúscula del apellido es del documento que se produce, no del que se lee.
- **El cierre es editable.** «Cómo va a quedar» ya no es solo una vista: el bloque verde
  del final es `contenteditable`, se escribe encima y con Ctrl+B se pone negrita. Una vez
  tocado, los campos dejan de pisarlo —se avisa con el fondo ámbar y «Cierre editado a
  mano»— y «Volver a generarlo» deshace. Al descargar, `segsDeDom()` arma los runs del
  Word leyendo ese HTML, así que lo que se ve es lo que va, con sus negritas. Es el mismo
  camino que usa DDJJ en su panel 3.
- «JSZip is not defined» explica ahora que no se pudo cargar el lector de Word, y la
  descarga dejó de afirmar que se descargó sin saberlo.

## Pendiente

- Sigue el diseño viejo, con la plantilla escrita en el código. Puede quedar así: la
  migración a plantilla-como-dato no lo tiene como prioridad.
