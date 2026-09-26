# Expropiaciones

Inscripción directa por avenimiento. Ley 6394, artículo 21.

- **Estado:** prototipo. Anda y está adentro de SIGEC, pero no es de uso diario.
- **Archivo:** `expropiaciones.html`
- **En SIGEC:** `id:"expro"`, grupo propio, color `#8A6A12` / `#D8B65A`.
- Solo lo usan los escribanos.

## No se llama SIIGENO, y no se llama Avenimiento

El nombre del producto es **Expropiaciones**. Llevaba la marca SIIGENO en el cartel, el
encabezado, el título de la ventana y dos pasos del tour: contradice de frente el reparto
que se decidió —SIIGENO resuelve el trámite externo, SIGEC es el sistema interno— y
encima la herramienta va adentro de SIGEC. Cero ocurrencias ahora.

## Lo que se sacó, y por qué

- **La lectura por IA.** Pedido del equipo, por costo: la herramienta ya no manda
  documentos a la API. Al sacarla se cayeron solas tres cosas más —el modal de
  "expediente digital o físico", que existía solo para elegir si se leía solo o a mano;
  la demostración guiada, escrita alrededor del escaneo; y el botón de API key.
  **Queda** adjuntar documentos y el checklist: no cuestan nada y sirven para la compulsa.
- **La firma digital.** Era decorativa y lo decía ella misma al pie. Estampaba un
  recuadro con sello al pie de cada PDF: produce algo que parece firmado sobre documentos
  que van al Registro. Sacada entera.
- **La compulsa de 30 tildes.** La etapa 4 tenía tres mecanismos de revisión apilados —35
  tildes para cerrar una etapa— y un botón "Verificar todo" cuyo propio cartel decía
  "Usalo solo si ya los compulsaste". Si hay un botón para tildar los treinta de una, los
  treinta tildes no están haciendo el trabajo. Ahora hay **un tilde por documento de
  origen**: matrícula, plano, resolución aprobatoria, convenio. La escribana atesta lo
  mismo. De 35 tildes a 9.
- **La columna Físico / Digital** de la bandeja, que mostraba un modo que ya no existe.
- **La columna IA / a mano** de cada dato: sin lectura automática todo es a mano.
- **Los tres expedientes inventados de la "Vista escribana"**, con apellidos y números
  que parecían de verdad. Ahora muestra solo el expediente abierto.

Lección que conviene no perder: **cada función que se sacó se llevó otras dos o tres.
No eran funciones de más: eran funciones que existían para sostener a otra.**

## La etapa 5 dejó de ser una sala de espera

Eran 3.194 px de alto y más de un tercio eran dos cajas grises de 960 px que decían
"Apretá «Generar documentos»". Ahora los documentos se generan al entrar a la etapa, la
caja vacía se encoge (`:has(.vacio)`) y el botón de abajo es "Descargar paquete (.zip)",
que es lo que de verdad cierra la etapa. De 3.194 a 1.246 px.

## Fechas

Los cinco `<input type="date">` son campos de texto con máscara `dd/mm/aaaa`. No era
hipotético: en un Chromium en inglés el campo decía **06/24/2025** mientras el control
automático de la misma pantalla escribía **"Convenio (24/06/2025)"**. Los casos guardados
con el formato viejo (`aaaa-mm-dd`) migran solos al abrir.

## Cómo guarda

- `localStorage`, con autoguardado. Las descargas —"Guardar caso" y el `.zip`— usan el
  **estado en memoria**, no el `localStorage`, así que salen bien aunque el
  almacenamiento esté lleno. Antes salían con la versión vieja o directamente `{}`.
- El ejemplo que carga el botón es inventado de punta a punta.
- `sigec_escribanas`: los nombres de las escribanas, compartidos con el rogador y con
  primer testimonio. **Los casos guardados antes de este cambio migran solos**: el valor
  del select era `NOMBRE|CARGO|REGISTRO|CORTO`, y si se dejaba así el select quedaba en
  blanco porque ese valor ya no existe entre las opciones. Se traduce al cargo. Verificado
  abriendo un caso con el formato viejo.

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

Acá además **el revisor se escribe**. Antes era un select con los cuatro nombres del equipo
puestos en el código; ahora es un campo de texto con las dos escribanas cargadas ofrecidas
en un `datalist`. El equipo cambia y no es código.

Y el control de la etapa de revisión avisa en rojo si falta el nombre, con el botón «Ir»
que lleva a cargarlo, como los demás controles.

## No llama a la API

Ya no. El guardia de hostname que tenía (`/claude\.ai$|anthropic\.com$|^$/`, cuyo `^$`
aceptaba el hostname vacío de `file://`, o sea el uso real) desapareció con la llamada.

## Cuatro pasos, y tres niveles de letra

Los pasos son **El caso y las personas · Polígono · Revisión · Documentos**. Los dos
primeros eran pantallas separadas y son el mismo trabajo —pura carga de datos—, así que
se unieron. Los índices de etapa están escritos a mano en `estadoEtapas()`, en
`irEtapa()` —que dispara la revisión y la generación de PDF por número—, en los avisos de
la revisión que llevan botón "Ir" y en la bandeja: si alguna vez se agrega o saca una
etapa, hay que corregir los cuatro lugares.

**Un documento a la vez, y los opcionales plegados.** Las tarjetas del paso 1 se abren de
a una: se toma el papel que se tiene en la mano, se carga y se pasa al siguiente con el
botón del pie. Cada tarjeta cerrada muestra su avance —"3 de 5", "completo ✓"—, así que
no hace falta abrirla para saber cómo viene.

Adentro, **los campos opcionales van plegados**: son once de los treinta y uno, o sea un
tercio del formulario. `OPCIONALES` ya existía para la compulsa y es la misma lista. Si un
opcional trae dato —al abrir un caso o el ejemplo— su bloque se despliega solo:
`abrirOpcionalesConDatos()`. Nada cargado puede quedar escondido.

De 31 campos a la vista a **11**. Las tarjetas de personas usan el mismo plegado, y
`abrirDoc()` solo cierra las del mismo bloque: abrir el representante no cierra la
matrícula.

**El paso 1 está agrupado por documento de origen, no por tema.** Eran dos tarjetas
—"Inmueble" y "Expediente y actos"— con treinta campos entre las dos, y la segunda era
un cajón de sastre con datos de tres documentos distintos. Ahora hay **una tarjeta por
papel**, numeradas: la matrícula, el plano, la resolución aprobatoria y el convenio. Se
toma un documento, se llena su tarjeta y se pasa al que sigue; nunca hay que barrer
treinta etiquetas buscando cuál corresponde.

Son **los mismos cuatro documentos que después se compulsan** en el paso 3, y la tarjeta
lo dice. Esa correspondencia es lo que hace la pantalla pedagógica: lo que cargás por
documento es lo que después atestás por documento.

La resolución aprobatoria trae trece datos de tres partes distintas, así que adentro de
su tarjeta hay tres subtítulos —qué obra y con qué ley, quién la dictó, la que
individualiza el inmueble— en vez de partirla en tres tarjetas, que rompería la
correspondencia con la compulsa.

La escribana que autoriza salió de ahí: no viene de ningún documento del expediente, es
una elección, y la tarjeta lo aclara.

La jerarquía de la pantalla son tres niveles, y cada uno es de **otra forma**, no solo de
otro tamaño, que era el problema: el bloque (`.bloque > .rotulo`, en mayúscula espaciada
y con una regla), el título de tarjeta (`h2`, versalita normal, color fuerte) y la
etiqueta de campo (`label`, mayúscula chica). Antes el título y la etiqueta eran
16,5px/700 contra 11,5px/600 —demasiado parecidos— y había cinco tarjetas iguales
seguidas sin nada que las agrupara.

## Lo que la herramienta NO hace, y es a propósito

- **No adjunta documentos.** Se sacaron la caja de arrastrar, el reconocimiento del tipo
  por el nombre del archivo, el checklist y el visor de PDF de la revisión. La compulsa
  se hace contra el expediente en papel que la escribana tiene en la mano.
- **No guarda ni abre casos en archivo.** El autoguardado en `localStorage` sigue, pero
  no hay exportar ni importar `.json`.
- **No tiene recorrido guiado ni bandeja.** La bandeja de verdad es por trámite y se hace
  aparte.
- **No llama a la API.**

## Pendiente

- **`modoExp` quedó fija en `'fisico'`** en vez de borrarla, porque los casos ya
  guardados la traen y `guardar()` la serializa. Cuando se rehaga conviene sacarla.
- Sigue el diseño viejo, con la plantilla escrita en el código.
- Donaciones, cuando se retome, conviene partirlo de acá: es muy parecido.

## Hecho el 15/09/2026

- **Tres pantallas rotas, heredadas del paso de cinco etapas a cuatro.** El botón «Ver» de
  la revisión llamaba a `irEtapa(4)`, y las pantallas son `s0..s3`: apagaba todas y dejaba
  la página EN BLANCO, sin aviso. Los cuatro «Corregir» llamaban a la etapa en la que ya
  estaban. Y los avisos de nacionalidad y estado de familia mandaban al Polígono, donde
  esos campos no existen.
- **El diseño del mockup, aplicado.** Se fue la barra arcoíris —seis colores, lo más
  llamativo de la pantalla, y no decía nada—; «Los papeles» dejó de verse igual que «N° de
  matrícula»; los pasos pasaron a una barra segmentada sin números, porque había dos
  numeraciones iguales; «0 de 5» usa los colores de estado; y la tarjeta abierta se
  distingue por el borde de acento.
- **El acento es violeta `#5E4B8B`, no el ocre del menú.** Ese ocre era exactamente el
  ámbar que en todo SIGEC significa «falta algo»: el título y el botón principal habrían
  parecido una advertencia permanente. El cambio va en `HERRAMIENTAS` de `sigec/index.html`.
- **Una colisión de cascada que se veía desde el principio:** `.screen{padding:14px 0 60px}`
  le ponía en cero los 22 px laterales de `.wrap` —misma especificidad, gana el que va
  después—, y por eso los rótulos de sección colgaban pegados al borde. Ahora `padding-block`.
- `setT` y `setC` escriben adentro de los PDF oficiales y tenían el catch vacío; ahora los
  campos que no entraron se nombran en rojo al generar.
- La alerta de almacenamiento lleno mandaba a «Guardar caso», un botón que ya no existe.
- La Escribana Sustituta iba sin número de registro, y ese valor se imprime en la Rogación.
- **La caja de la vista previa mentía al revés:** el documento se generaba y la caja seguía
  diciendo «Se genera al entrar a esta etapa». Se decide por el protocolo —`file:` desde la
  carpeta, `https` publicado—, porque esperar el evento `load` no sirve: cuando el
  navegador bloquea el PDF igual carga SU página de bloqueo y `load` dispara igual.

## Pendiente, además de lo de abajo

- **Nunca se verificó que los formularios se llenen de verdad.** `pdf-lib` viene de cdnjs y
  en el entorno donde se programa está bloqueado. La prueba válida es abrir el ZIP desde la
  carpeta y llegar al paso 4.
- **Rediseño de la etapa 1, pedido por el equipo el 15/09 y todavía sin hacer:**
  los «datos que no siempre están» del plano y la resolución individualizadora siempre
  están; los titulares registrales salen de la matrícula y los comparecientes del convenio,
  así que «Las personas» debería disolverse y cada grupo mudarse a su documento —un solo
  eje: por documento—; el orden correcto es matrícula · plano · ley y resolución
  individualizadora · convenio y resolución que lo aprueba; y hay que prever el caso sin
  plano, cuando se expropia la matrícula completa.
- **El polígono tiene que entrar en media pantalla** —el expediente digital vive en la otra
  mitad— y conviene un visor del plano adentro, plano a la izquierda y planilla a la
  derecha. Desde la carpeta funciona; en la copia publicada no.
