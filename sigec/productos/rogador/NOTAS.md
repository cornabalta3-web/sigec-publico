# Rogador

Rogación de inscripción para el Registro General de la Provincia, a partir de la
escritura.

- **Estado:** en uso.
- **Archivo:** `rogador.html` — es la **versión 2** (la 1 quedó en el historial de git).
- **En SIGEC:** `id:"rogador"`, grupo Escrituración, color `#A81419` / `#E2685F` en
  oscuro. El rojo es el del sello; tampoco es decorativo.

## Cómo guarda

- `localStorage` — `rogador_key` (la clave de la API, se guarda al tipear),
  `rogador_model` (el modelo elegido) y `sigec_escribanas` (los nombres de las escribanas,
  compartidos con primer testimonio y expropiaciones).
- **IndexedDB** — base `rogador`, almacén `kv`, clave `tpl`: el formulario PDF que carga
  el usuario. Es la única herramienta que no guarda todo en `localStorage`.

## Llama a la API

Sí, es la otra de las dos. Y trabaja sobre escrituras y rogaciones, que son datos
distintos de los de las DDJJ y con otra base legal.

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

Acá el campo va al lado del selector, en el punto 1 de la revisión, y se guarda mientras se
escribe **sin redibujar el formulario**: la regla de no romper el foco vale igual que para
las máscaras. Al cambiar el cargo, el campo muestra el nombre de ese cargo. Si falta, el
campo queda en rojo y el generador de PDF lo suma a la lista de fallos, que ya existía.

## Reglas propias

- **Los números salen siempre en cifras.** La escritura los escribe en letras porque así
  lo manda la regla notarial; la rogación es un formulario de renglones cortos y
  "trescientos uno" no entra donde entra "301". Lo hace `normalizarCifras()`, que corre
  sobre lo que devuelve el lector local y sobre lo que devuelve la IA.
  - `cifra()` solo toca el valor si no tiene ningún dígito, así "224,84 mts 2" y
    "1.234.567" quedan intactos.
  - El domicilio no se puede convertir palabra por palabra: "un" y "una" también son
    artículos. Por eso `cifrasTrasEtiqueta()` convierte solo lo que sigue a "Lote",
    "Mza" o "Manzana".
  - La regla 14 del prompt dice lo mismo, para que la IA ya devuelva cifras.
- **El `\b` de la línea del cardinal es seguro acá**, aunque la regla general del
  proyecto lo marque como peligroso con vocales acentuadas: `norm()` normaliza y quita
  los acentos de los dos lados antes de armar la expresión. No tocar.
- **Las notas se fusionan, no se reemplazan.** "Completar con IA" borraba todas las
  notas locales, incluida la del apellido dudoso, y al ver cero notas la pantalla
  imprimía el cartel verde "Salió todo de la escritura, sin huecos": el caso que había
  que avisar terminaba mostrando luz verde.
- **`PDFLib` se lee adentro de las funciones que lo usan**, nunca en el nivel superior.
  Cuando estaba arriba, un CDN que no contestaba tiraba `ReferenceError` y cortaba el
  script entero: la herramienta quedaba en blanco.

## Pendiente

- **Detección de adjudicatarios cuando el apellido no viene en mayúsculas.** Hoy la v2
  parsea "Mariana Beatriz Ayala" y emite la nota, pero falta tomar el nombre desde "a
  favor de la señora…" —esa frase hoy solo confirma apellidos ya detectados— y "por otra
  parte la señora…" no se usa en absoluto. Si falta el bloque de nombre + nacionalidad +
  nacimiento + DNI, devuelve cero adjudicatarios.
  Regla general detrás: **la mayúscula del apellido es del documento que se produce, no
  del que se lee.** Ninguna detección debería apoyarse en el formato del texto de entrada.
- **Casos de prueba que faltan en el repo:** `Minuta_escritura_Ayala.docx` y
  `Primer_Testimonio_-_ESC_XX_-_Mariana_Beatriz_Ayala.docx`. Con esos dos tienen que
  salir adjudicataria, superficie y titular registral.
- **Migrar el modelo tiene una trampa:** el modelo no está en el código sino en un
  `<option>`, y la elección se persiste en `rogador_model`. Si se cambia el `value` del
  option, las máquinas que ya tienen guardado el valor viejo asignan uno que no existe y
  el select queda en blanco. Hay que migrar el valor guardado, no solo el option.
- Revisar si hay ejemplos con datos reales en el prompt, como los había en DDJJ.

## Hecho el 15/09/2026

- **Pasó a `claude-sonnet-5`**, más nuevo y más barato que `claude-sonnet-4-6` (2/10 contra
  3/15 por millón). Y **se migró el valor guardado**, que era la trampa anotada abajo: las
  máquinas que tenían elegido el modelo viejo dejaban el select EN BLANCO y la llamada
  salía sin modelo. Ahora el valor viejo se traduce y cualquier valor desconocido vuelve a
  la primera opción. Verificado con los dos casos.
- **El llenado del PDF se comía todos los errores.** `T` y `C` escriben adentro del
  formulario oficial del Registro y tenían el catch vacío: si un campo cambiaba de nombre
  salía en blanco y nadie se enteraba hasta tener el papel. Ahora se juntan en `fallos` y
  la descarga los nombra.
- **El recorte a dos adquirentes era mudo.** El tercero desaparecía de un documento que va
  al Registro; ahora deja nota con los dos que quedaron.
- **El código postal sigue siendo 9000 fijo**, pero al menos avisa cuando el domicilio del
  primer adquirente no parece de la capital, que es cuando está mal.
- La descarga dejó de decir «Rogación descargada» sin saber si el navegador la aceptó.

## Pendiente, además de lo de abajo

- El CP fijo hay que resolverlo de verdad: o sale de un campo cargado, o del domicilio.
- La detección de adquirentes sigue necesitando el bloque completo de nombre +
  nacionalidad + «nacido el» + DNI. Si falta, devuelve cero. Los dos documentos de Ayala
  siguen sin estar en el repo, así que el caso real sigue sin probarse.
