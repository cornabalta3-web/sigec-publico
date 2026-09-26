# Escrituras sociales

Minuta base por manzana y confección por lote. Ley 9811.

- **Estado:** prototipo. **Nunca presentarlo como herramienta en uso.** Se implementó
  para algunas escrituras de Colonia Tirolesa y funcionó muy bien, pero no es de uso
  diario.
- **Archivos:**
  - `escrituras-sociales.html` — **el producto entero**, el que abre el menú de SIGEC.
    Barrio → lotes → minuta base del barrio → minuta de cada lote. Es la fusión de lo
    que antes eran `barrios.html` y `armador-minutas.html`.
  - `armador-minutas.html` — **el armador viejo, suelto. No está enlazado desde ningún
    lado.** Queda como red de seguridad mientras se prueba la versión fusionada; usa su
    propio `localStorage` (`armadorMinutas`), así que no se pisa con el nuevo. Cuando la
    fusión esté probada con expedientes reales, se borra.
  - `minuta-colonia-tirolesa.html` — el generador con la plantilla de ese barrio ya
    escrita. No está en el menú.
  - `prueba-carpetas.html` — la prueba de si el navegador entrega la estructura de una
    carpeta elegida a mano. Sin correr todavía.
- **En SIGEC:** `id:"sociales"`, grupo Escrituración, color `#B5533C` / `#E08A6F`.
- Solo lo usan los escribanos, no todo el equipo.

## Es el modelo de diseño del proyecto

**La plantilla es un dato, no código.** Quien arma la base pega una escritura ya hecha,
selecciona los tramos variables y los marca; hay detección automática que propone y el
usuario confirma. Cada auxiliar entra a su lote y completa solo los datos de ese lote.
Si la minuta cambia, se cambia la base y los datos cargados no se pierden.

## El armador dejó de ser otra herramienta (17/09/2026)

Pedido del equipo: *"al momento de confeccionar la minuta base eso tiene que estar
integrado en escritura social, no es como que tener que armar ir un botón y hacerlo desde
afuera, o sea como que tiene que ser todo el mismo sistema de escritura social"*.

Tenía razón, y no era solo de presentación. **La minuta base es del barrio** —el barrio
guarda el estudio de título y la minuta base, está escrito así desde que se definió el
modelo— y **la minuta individual es del lote**. Que vivieran en otro archivo obligaba a:

- exportar un `.json`, dejarlo en el disco R y que cada uno se acordara de cargarlo;
- volver a tipear lote, manzana, adjudicatarios, expediente y resolución, que ya estaban
  cargados en el tablero;
- guardar las escrituras en una lista suelta del navegador (`armadorMinutas`), sin
  relación con el lote que les corresponde.

Qué cambió, concretamente:

- **`b.base`** — la minuta base, guardada adentro del barrio. Se entra con «Armar la
  minuta base» desde la tira del barrio, se guarda con un botón, no hay archivo que
  dejar en ninguna carpeta. La descarga `.json` sigue existiendo, pero como respaldo.
- **`l.datos`** — la minuta individual, guardada adentro del lote. Se entra con el botón
  «Minuta» de cada fila. **Se guarda sola mientras se escribe** (a los 0,9 s de la última
  tecla), que era lo único que hacía perder trabajo de verdad.
- **`b.fijos`** — lo que es del barrio y no del lote (localidad, departamento,
  nomenclatura, año de la base, sección, escribana) queda puesto para el lote siguiente.
- **Prellenado desde el tablero:** lote y manzana pasados a letras, barrio, expediente,
  resolución, fecha de firma como fecha del acto, fecha del notarial, y los
  adjudicatarios parseados de la carpeta (`APELLIDO Nombre y APELLIDO Nombre`).
  **Si el apellido no viene en mayúsculas no se adivina: se deja una nota en pantalla**,
  que es la regla general del proyecto —la mayúscula es del documento que se produce, no
  del que se lee.
- **El tablero muestra el estado de la minuta** en la fila del lote: «empezada, sin
  revisar» o «revisada el dd/mm/aaaa». No hay nada extra que cargar: sale de lo que ya
  se hizo.
- **Al guardar una minuta, el lote pasa a «En confección»** si estaba en «Designado» o
  «Sin designar».
- **La base la edita quien la tiene designada** (o la escribana). El resto la ve en solo
  lectura, que es lo que hace falta para entender por qué la individual dice lo que dice.
- **Una base sin ninguna marca ya no dice «completa».** Antes devolvía el mismo texto
  para todos los lotes y la pantalla lo daba por terminado. Ahora avisa.
- **Se fueron `alert`, `confirm` y `prompt`** del armador: adentro del marco de SIGEC el
  navegador los ignora, así que el aviso no aparecía y el borrado se cancelaba solo.
  Usan el diálogo y la barra de aviso del tablero.
- **Se fue `window.open`**: la copia publicada lo bloquea en silencio, y ya no hace falta
  porque el armador está en la misma página.
- Imprimir: la carátula y la minuta salen de la misma página, y la clase `impMinuta` del
  `body` dice cuál de las dos se está imprimiendo.

### La segunda pasada: que además lo parezca (17/09/2026)

el equipo probó la primera versión y dijo: *"no fusionaste nada. Aprieto el botón y me abre
de vuelta el armador de minutas. Tienen que tener la misma identidad visual, todo. Aprieto
y dice volver a SIGEC. No tiene que salir nunca de SIGEC."*

El código estaba fusionado —cero `window.open`, cero referencias a otro archivo— pero **la
pantalla no**. Al entrar a la minuta se apagaban de un saque la cinta de SIGEC, el
encabezado con el nombre del producto y la franja, la columna se ensanchaba de 1080 a
1340 px, el acento se daba vuelta de terracota a azul marino, y la píldora flotante
"← Volver a SIGEC" seguía visible. Era la misma página diciendo que era otra.

Qué cambió:

- **La cáscara no cambia nunca.** La cinta de SIGEC y el encabezado con «Escrituras
  sociales» quedan siempre en pantalla. Lo que cambia es el renglón de abajo —el camino:
  *Villa Los Robles › Lote 03 · Mza 12 › SUÁREZ Marta Elena*— y los botones de la derecha.
  El encabezado ahora es pegajoso, así que el modo de volver nunca se pierde al bajar.
- **Un solo ancho** (1180 px) y una sola franja: la de 3 px pasó a ser el borde del
  encabezado, para que no se despegue al scrollear.
- **Un solo acento.** Se fue el token `--titulo`, que era `--navy` con otro nombre y solo
  lo usaba el armador. Botón principal, títulos de tarjeta, cabecera del panel, rótulos de
  grupo y el contador de la revisión: todos terracota.
- **Una sola forma.** Tarjetas de 11 px y borde 1,5 px; botones del tablero; avisos con la
  barra de color a la izquierda, como `.alerta`; las marcas y las fichas con forma de
  píldora, como `.et`.
- **Adentro de la minuta no hay ninguna salida.** La píldora "← Volver a SIGEC" se oculta:
  se vuelve al barrio, y recién de ahí a SIGEC. Y la píldora dejó de estar pintada con un
  azul fijo: usa los tokens, así que también se lee en modo oscuro.
- **Contraste en oscuro:** `button.p` y las cabeceras de color pasaron de `#fff` a
  `var(--pap)`, que es blanco en claro y casi negro en oscuro. En oscuro el acento es un
  salmón claro y el texto blanco encima casi no se leía.
- **Un defecto de render que nadie había visto:** la lista de coincidencias de la detección
  automática envuelve cada fila en un `<label>`, y heredaba la regla de rótulos del tablero
  —10,5 px, MAYÚSCULAS y gris—. Salía ilegible. Ahora tiene su propia clase.
- El panel de la derecha dice **«Cómo va a quedar»** en vez de repetir el nombre del lote,
  que ya está arriba. `nombreMinuta()` sigue sirviendo para el nombre del `.docx`.
- En la pantalla de prueba había **dos flechas de volver** distintas en la misma barra.
  Quedó una.

Verificado en Chromium desde `file://`, claro y oscuro, suelto y adentro del menú: el
nombre del producto y la cinta no se van en ninguna de las tres pantallas, la píldora
aparece solo en el tablero, el ancho no salta, la lista de coincidencias se lee, el
contraste en oscuro da, las dos impresiones siguen saliendo bien, sin desborde horizontal
a 400 px y sin un solo error de JS.

### Primera tanda de prueba real, del equipo (18/09/2026)

Nueve cosas marcadas probando con un expediente de verdad. Las nueve, hechas:

1. **Cerrar el alta sin querer ya no borra lo cargado.** El alta de barrio y la de lote
   guardan un borrador tecla por tecla, en `sociales_borradores`. Al volver a abrirlas
   siguen donde estaban, con un cartel y un botón «Empezar de cero». Se limpia al guardar.
   Sobrevive a cerrar la pestaña.
2. **El auxiliar carga sus propios lotes y quedan designados a él.** Están las dos
   opciones: la escribana reparte con «Designar», o el que tiene el expediente individual
   lo carga y se autodesigna. Para eso hizo falta corregir algo de fondo: **el auxiliar ve
   todos los barrios activos**, porque si no, no tiene dónde cargar. Lo que sigue sin ver
   son los lotes de los demás — «solo lo tuyo» es sobre los lotes, no sobre la existencia
   del barrio. Los barrios donde no tiene ningún lote aparecen plegados, con «+ Mi lote» a
   la vista, y ese plegado es de quien mira, no del barrio.
3. **La carpeta del lote se ve desde el lote.** Misma lógica que el explorador de Windows:
   «Ver la carpeta» abre el selector, y queda anotado en `l.archivos` qué tenía, cuándo y
   quién la miró — así lo ve también el que no la tiene a mano. Se listan los documentos
   que suele llevar (minuta, resolución, DNI, CUIL, notarial, catastral) y, aparte, todo lo
   demás que haya. Respeta la regla: lo que no se reconoce **no se da por faltante**, se
   dice que puede ser que el archivo se llame distinto.
   **De paso queda contestada la pregunta abierta de `prueba-carpetas.html`:** el navegador
   sí entrega la lista cuando el usuario elige la carpeta. Verificado en Chromium acá;
   falta confirmarlo desde el disco R, que es donde importa.
4. **La minuta base ya no bloquea la carga.** Estaba al revés: los datos son del lote y la
   base es del barrio, y son independientes. Ahora se entra igual, se carga todo, y el
   texto se arma solo el día que la base esté, con lo que ya estaba cargado.
5. **El lote y la parcela son el mismo número.** Se fue el campo «Parcela»; `{PARCELA}`
   toma el valor del lote.
6. **Sí, queda guardado.** La minuta se guarda sola a los 0,9 s de la última tecla, y desde
   ahora el alta también. Verificado cerrando y reabriendo.
7. **El número de escritura pasó al final,** a una tarjeta «Al protocolizar», y **dejó de
   contar como dato faltante**: sale del protocolo, al imprimir. Si la base lo pide, se
   avisa aparte.
8. **La vista se dice en pantalla.** Ficha en el encabezado —«Vista de la escribana» en
   azul, «Vista de Ávila» en terracota— y la cinta de arriba cambia de color y de texto.
   Confundir una vista con la otra es creer que faltan lotes que están.
9. **«No falta nada» era mentira, y la causa explica también el punto 7 de la pregunta.**
   Todo lo que la pantalla contaba dependía de las marcas de la minuta base. Con una base
   de dos marcas, cargar veinte datos daba «no falta nada» — y ninguno de esos veinte
   aparecía en «Cómo va a quedar», porque no había dónde ponerlos. Ahora:
   - el cartel verde dice **«Está todo lo que pide esta minuta base — son N marcas»**, que
     es lo único que el contador puede afirmar, y agrega que la revisión sigue siendo
     obligatoria;
   - y aparece un aviso ámbar con **los datos cargados que no tienen marca en la base**,
     nombrándolos uno por uno, con la salida: marcarlos en «Ver la minuta base».
   La ficha de arriba muestra lo peor que pasa, no lo mejor.

Los barrios guardados antes migran solos: `b.minuta` —que era el nombre del archivo del
disco R— se sigue mostrando como aviso hasta que se cargue la base de verdad, y desde la
pantalla de la base se puede abrir ese mismo `.json`.

Verificado en Chromium desde `file://`, claro y oscuro, suelto y adentro del menú:
ejemplo cargado, entrar a un lote, prellenado correcto (lote «tres», manzana «doce»,
SUÁREZ / Marta Elena, sexo femenino), escribir y volver, recargar y que siga, marcar a
mano, detección automática (16 coincidencias), guardar la base, el caso del barrio sin
base, el del apellido dudoso, los dos roles, las dos impresiones, sin desborde horizontal
a 400 px y sin un solo error de JS.

## Colonia Tirolesa no está en esta copia

Esa herramienta lleva a los transmitentes del barrio dentro de la plantilla —comparecen
en todas las escrituras de ahí—, así que no se puede anonimizar sin romper el documento.
Vive solo en la copia interna.

`BASE_MODELO` del armador **es inventado**.

## Los tres motores de concordancia ya divergieron

| archivo | función | concordancias |
|---|---|---|
| `escrituras-sociales.html` | `P()` | 16 — el completo, el que conviene reusar |
| `minuta-colonia-tirolesa.html` | `P()` | 15 — le falta `derecho`, y renombra tres (`DICE`, `doc1`, `declBenef`) |
| `demos/siigeno-productos-nuevos.html` | `P()` | el de escrituras sociales |

No son intercambiables tal como están. Unificarlos es trabajo real.

Las listas de excepciones también divergieron:

| | `armador-minutas.html` | `minuta-colonia-tirolesa.html` |
|---|---|---|
| `EXC_F` | 32 nombres | 34 — agrega `BRISEIDA` y `YAMIL` |
| `EXC_M` | 11 — incluye `ELIAS` y `JOSE` | 9 |

De las dos que agrega Colonia Tirolesa, **una no hace nada y la otra es sospechosa**:
`BRISEIDA` termina en A, así que ya daba femenino sin estar en la lista; `YAMIL` termina
en L, así que sin la excepción daría masculino, que es lo habitual para ese nombre.
**A confirmar con el equipo antes de unificar**, porque al unificar hay que elegir una.

## La resolución es un dato del lote, no un nivel

Decisión del equipo, mirando cómo está el disco R. La carpeta compartida es
`ESCRITURAS SOCIALES\ BARRIO \ adjudicatario`, más una carpeta `BARRIOS FIRMADOS` donde
se mueve el barrio cuando termina. **No hay ningún nivel de resolución en el medio**, y
el armador tampoco lo tiene: `{RESOLUCION}` es una marca de dato que se completa por
expediente individual. O sea que la resolución nunca fue un contenedor.

La pantalla lo tenía como nivel y dejó de tenerlo: el barrio tiene lotes, y cada lote
lleva el número de su resolución como un campo más, al lado del expediente. Se ganan tres
cosas: la pantalla queda igual que la carpeta de Windows, desaparece un nivel entero de
interfaz —crear resolución, editarla, designar por resolución—, y **los lotes se pueden
ordenar por urgencia de punta a punta**, cosa que agrupados por resolución no se podía.

De qué tanda es cada uno se contesta mirando la columna. Y "Barrio firmado" se llama así,
como la carpeta, no "terminado".

Los casos guardados con el formato viejo **se aplanan solos al abrir**: `migrar()` copia
el número de cada resolución a sus lotes y borra el nivel.

## El barrio es el contenedor, no el expediente madre

```
BARRIO  Campo de la Ribera        ← estudio de título + minuta base
  Resolución 412/2025             → 11 individuales
  Ampliación 512/2026             →  4 individuales nuevos
```

`{RESOLUCION}` es una marca de dato, no parte de la plantilla, así que una ampliación no
obliga a tocar la minuta base: se dan de alta los individuales nuevos. Si se modelara por
expediente madre, un barrio que vuelve aparecería dos veces con la minuta duplicada.

**Un expediente individual es exactamente un lote**, y un lote puede tener más de un
adjudicatario.

## Lo que mueve el proceso es la fecha de firma

Decisión del equipo, y reordena la pantalla entera: el trámite se guía por la **fecha de
firma**, que se pone cuando llega el expediente madre o después, según cuál esté en
proceso. Por eso los lotes se ordenan por esa fecha, del más próximo al más lejano, y los
que todavía no tienen fecha van al final: no entraron al calendario.

Y cambia el aviso. Antes decía "el notarial vence en N días", que es cierto pero no es el
riesgo. **El riesgo es que el notarial venza ANTES de la firma**, y eso es lo que ahora
dice: compara el vencimiento —30 días desde que el certificado vuelve contestado— contra
la fecha de firma.

## Dos vistas, porque son dos preguntas distintas

La escribana y los auxiliares no miran lo mismo, y no es cuestión de permisos: tienen
preguntas distintas. La escribana pregunta *"qué hay que repartir y cómo va todo"*; el
auxiliar pregunta *"qué tengo que hacer yo hoy"*. Una sola pantalla que conteste las dos
contesta mal las dos. El selector de arriba cambia de vista y se guarda en
`sociales_rol`.

**Quiénes están en la lista.** `EQUIPO` (línea 254) son hoy **Ávila y Ferreyra**.
el equipo las sacó el 14/09/2026, y el 15 quedó aclarado por qué: **Vera no existía** —un
nombre que se había colado en `CLAUDE.md`— y **la Escribana Sustituta no es una auxiliar**;
figuraba dos veces porque el apellido estaba escrito de dos maneras. `EQUIPO` son
los auxiliares a los que se les designan lotes, así que las escribanas no van ahí.

- **Vista de la escribana.** Arriba, lo que no puede avanzar hasta que ella lo reparta o
  lo revise: barrios sin designar quién arma la minuta, lotes sin designar, lotes
  esperando su revisión. Abajo, todos los barrios con todos los lotes.
- **Vista de un auxiliar.** Solo sus lotes y el barrio donde él arma la minuta. Arriba,
  lo suyo: la minuta que le designaron, sus lotes observados, los que tiene para
  confeccionar. No puede designar, ni observar, ni borrar.

**Esto resuelve la asimetría que hacía dudar de toda la pantalla.** El que carga el dato y
el que se beneficia eran personas distintas — pero cuando la escribana designa, está
haciendo un trabajo que ya hace, y el tablero queda cargado como subproducto. Es el mismo
truco de la carátula impresa.

## Observar: dos alcances distintos

- **Observación del lote** — un error puntual de ese expediente. Escribirla deja el lote
  en «Observado» y le aparece al responsable en su lista. Borrarla lo devuelve a «En
  confección». Escribir la observación *es* marcar Corregir: no hacen falta dos acciones.
- **Observación de la minuta base** — un error general, que toca a **todos** los lotes del
  barrio. Se escribe en el barrio y se muestra arriba de la lista, en rojo, para todos.
  Solo la escribana la levanta.

Los estados son los del circuito, con la designación adelante: sin designar · designado ·
en confección · en revisión · observado · listo para firmar · firmada.

## La pantalla del barrio

Es la mitad que faltaba: el armador sabía hacer la minuta, pero nadie sabía en qué anda
cada expediente. Tiene exactamente **dos funciones**, que son las dos que le devuelven
algo a quien carga el dato:

1. **Imprimir la carátula.** Es el mismo llenado de hoy, no trabajo extra: escribana,
   responsable, fecha de inicio, carpeta (el adjudicatario) y expediente se cargan una vez
   y salen impresos. **La grilla de informes sale vacía**: se sigue tildando a mano sobre
   el papel. Sin doble carga en ningún lado.
2. **El aviso de los 30 días.** Un solo dato más —cuándo volvió contestado el notarial— y
   evita que el certificado se venza antes de la firma. Es lo más útil que puede hacer el
   tablero.

Y el tablero de estados no se construye: **aparece solo**, porque esos datos ya quedaron
cargados.

Reglas que respeta, y por qué:

- **El contenedor es el barrio, no el expediente madre.** Una ampliación es dar de alta
  lotes en un barrio que ya existe; la minuta base no se toca.
- **La herramienta nunca pide ni envía sola.** Los 30 días arrancan cuando el certificado
  vuelve contestado, y guardar el borrador sin enviar retrasa ese reloj a propósito. Quién
  aprieta enviar y cuándo lo decide la persona, en la página del Registro.
- **La pantalla muestra lo que tiene que pasar, no todo lo que existe.** Los barrios
  terminados quedan plegados en una línea; cuando llega la ampliación se despliegan con
  todo. Fue la sobrecarga de datos lo que hundió la demo anterior.
- **Alta a mano.** No lee carpetas ni extrae datos de PDF. La presencia de un archivo no
  dice que el paso esté hecho —la minuta base se guarda en cada expediente esté lista o
  no—, así que deducir la etapa del listado daría por avanzado lo que no lo está.
- Guarda en `localStorage` bajo `sociales_barrios`, y la copia `.json` sale **del estado
  en memoria**, para que salga bien incluso cuando el almacenamiento falló, que es
  justamente cuando más se necesita.

El ejemplo que carga el botón es inventado de punta a punta.

## Pendiente

- Pasarlo de prototipo a operativo: base e individuales, con alta manual. Es la
  prioridad 2 de diciembre.
- La demo `demos/siigeno-productos-nuevos.html` tiene prototipado el expediente madre
  con individuales colgando. Escrituras sociales no hay que diseñarlo: hay que promover
  eso. Pero no es un paso corto: está escrita para mostrar, con camino feliz y datos
  inventados.

## Hecho el 15/09/2026

- **La luz verde falsa, que era el peor defecto del producto.** Si ningún lote tenía
  cargada la fecha del notarial o la de firma, `pintarAlerta()` imprimía en verde «Ningún
  certificado notarial llega vencido a la firma» — la función principal afirmando algo que
  no podía saber. Ahora hay un tercer estado, ámbar, que dice cuántos lotes no se pueden
  evaluar todavía; y cuando sí hay riesgos, el rojo aclara de cuántos no se sabe.
- **El auxiliar podía hacer cosas que no le tocan:** marcar su lote «Listo para firmar» y
  «Firmada», y borrar lotes firmados. Ahora mueve su lote solo entre confección y revisión.
- **«Barrio firmado» prometía plegarse y no se plegaba** para ningún barrio creado en la
  app: nacen con `plegado:false` y la condición solo actuaba si era `undefined`.
- **Manda el adjudicatario.** La fila del tablero se ancla en los adjudicatarios, con ícono
  de carpeta, y el lote y la manzana bajaron a columna apagada. El texto de la observación
  salió de la tabla: queda el chip «Observado» y un contador «N observados» arriba. El
  tablero dice que hay una observación; el panel dice cuál.
- **La observación quedó firmada** —`obsQuien` y `obsFecha`— y el auxiliar tiene «Lo
  corregí», que devuelve el lote a revisión. Antes era un cartel pegado que alguien tenía
  que acordarse de sacar.
- **El armador:** se fue «Verificar todo», que tildaba todos los ítems de una; la pantalla
  dejó de prometer que la descarga estaba trabada cuando no lo estaba; y `BASE_MODELO`
  tenía todavía dos matrículas, el loteo, el expediente y el escribano autorizante reales.
- **«Abrir el armador de minutas» no hacía nada** en la copia publicada: `window.open`
  devuelve `null` cuando el navegador bloquea las ventanas. Ahora lo dice, y desde la
  carpeta abre como siempre.
- El equipo son **Ávila y Ferreyra**: Vera no existía y la Escribana Sustituta no es
  una auxiliar.

## Pendiente, además de lo de abajo

- La prueba de carpetas **anda** (confirmado por el equipo el 15/09): el navegador entrega
  la estructura desde el disco R. Eso habilita sacar la identidad de los nombres de
  carpeta y dejar el alta a mano como excepción. Es el próximo trabajo grande del producto.
- Falta decidir el panel del lote: el mockup eligió tabla con jerarquía + panel al costado;
  queda por implementar la vista de «lo que hay en la carpeta».
