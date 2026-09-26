# DDJJ

Escritura de protocolización de la declaración jurada patrimonial de funcionarios,
a partir del PDF que presenta la persona.

- **Estado:** en uso, todos los días. Es la herramienta más usada del sistema.
- **Archivo:** `ddjj.html` (antes se llamaba "amanuense"; el nombre viejo no va más).
- **En SIGEC:** `id:"ddjj"`, grupo Declaraciones juradas, color `#144D83` / `#5BA3E0` en oscuro.

## Cómo guarda

- `localStorage` — `escrituras_historial`: las últimas **50** escrituras (`HISTORY_MAX`).
  La 51 desplaza a la más vieja. Si el almacenamiento está lleno recorta a 25 y
  reintenta una vez; `saveToHistory` devuelve `ok`, `recortado` o `fallo` y quien la
  llama avisa en los dos casos malos. No hay guardado callado.
- `localStorage` — `escrituras_api_key`: la clave de Anthropic, se guarda con botón.

## Llama a la API

Sí. Es una de las dos herramientas que lo hacen (la otra es el rogador).

Los dos ejemplos de estilo del prompt del sistema —`MODELO_EJEMPLO_1` y
`MODELO_EJEMPLO_2`— **están anonimizados**. Antes eran dos declaraciones juradas
auténticas de dos funcionarios identificados, y viajaban a la API en cada pedido lo
hiciera quien lo hiciera. Si alguna vez se reemplazan, que sigan siendo inventados: lo
que el modelo aprende ahí es la forma de la prosa, no los datos.

## Números

- Medido: alrededor de **100 declaraciones** procesadas.
- Medido: una declaración llevaba una o dos horas y hoy lleva **ocho minutos**, tres de
  confección y cinco de corrección.
- Los cinco minutos de corrección **son la revisión obligatoria**. La herramienta no la
  eliminó: la abarató.

## Pendiente

- Es la primera en la cola para migrar a plantilla-como-dato (decisión 7), por uso y
  por impacto.

## Hecho el 15/09/2026

- **Los márgenes del `.docx` eran los de Word, no los notariales.** `w:pgMar` salía 1417
  twips en los cuatro lados (2,5 cm parejos). Ahora: arriba 1984 · derecha 964 · abajo 340
  · izquierda 3005, que son 3,5 / 1,7 / 0,6 / 5,3 cm.
- **El historial archivaba el borrador, no la escritura que se firma.** `saveToHistory` se
  llamaba apenas contestaba la API, antes de que el usuario tocara el panel 3 —que la
  propia pantalla lo invita a editar—, mientras la descarga leía `#doc-preview`. Ahora la
  entrada se guarda con `id` y `borrador:true`, y al descargar `actualizarEnHistorial()` la
  reemplaza por el texto corregido y el nombre final. El chip «borrador» avisa cuál quedó
  a medio camino.
- **El bloque del tema mataba el script entero** si el navegador bloquea `localStorage`
  (incógnito, datos de sitio bloqueados): la lectura estaba en el nivel superior, fuera de
  try/catch. La página se dibujaba completa y ni un botón respondía, sin un solo cartel.
- `getHistory()` se comía el error del JSON y decía «Todavía no generaste ninguna
  escritura», que es falso; ahora distingue vacío de ilegible.
- Borrar del historial escribía sin try/catch; el nombre del archivo se interpolaba sin
  escapar dentro de `innerHTML`; y «JSZip is not defined» explica qué pasó.
- **La anonimización tenía un agujero:** las constantes eran ejemplos inventados, pero la
  frase del prompt que las presenta seguía nombrando a dos declarantes y diciendo «son
  de casos reales anteriores», y viajaba a la API en cada escritura. Corregida.

## Pendiente, además de lo de arriba

- `claude-sonnet-4-6` sigue en el código de redacción: conviene pasarlo a `claude-sonnet-5`
  —más nuevo y más barato, 2/10 contra 3/15 por millón— como ya se hizo en el rogador. No
  se manda `temperature`, `top_p` ni prefill, así que es una línea; pero si alguna vez el
  modelo se persiste en `localStorage`, hay que migrar el valor guardado.
- El `cache_control` del prompt de extracción no cachea nunca: son ~800 tokens y el mínimo
  de Haiku 4.5 es 4.096. El de redacción sí funciona.
- El logo va incrustado en base64: 296 KB de los 382 del archivo, y `sigec/egg-logos.png`
  ya existe al lado.

## El inciso lo elige la escribana (15/09/2026)

En el paso 2, arriba del listado, hay dos opciones para la fórmula de identificación del
compareciente, con la **"b" marcada por defecto**:

- **inciso "b"** — *persona de mi conocimiento según el artículo 306 inciso "b" del Código
  Civil y Comercial de la Nación, doy fe.-*
- **inciso "a"** — *persona a quien identifico conforme el artículo 306 inciso "a" del
  Código Civil y Comercial de la Nación con el documento idóneo mencionado que me exhibe,
  doy fe.-*

Antes **el modelo elegía**: el prompt tenía una regla que le decía que replicara lo que
mostrara el PDF de referencia y, si no traía nada, usara la "b". Eso está mal de raíz —
cómo conoció la escribana al compareciente es una decisión suya, no un dato que se lea de
un formulario. Ahora la fórmula va literal desde `FORMULA_INCISO`, el prompt la recibe ya
elegida y tiene una regla explícita de que no puede cambiarla aunque el PDF muestre otra.

Las dos fórmulas son texto notarial y van tal cual: no se arman por plantilla. Si alguna
vez cambian, se cambian ahí y en ningún otro lado.
