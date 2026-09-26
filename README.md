# SIGEC

Herramientas de trabajo de una Escribanía de Gobierno. Son archivos HTML autónomos: se
abren con doble clic, sin instalar nada y sin servidor. Guardan en el navegador y exportan
a `.docx`, `.pdf` o `.json`.

**Esta es una copia para mirar.** Los nombres, los domicilios, los documentos, las
matrículas y los expedientes que aparecen en los ejemplos son inventados. La copia que se
usa vive en un disco de red de la oficina y no se publica.

## Cómo se abre

Bajá el repositorio, entrá a `sigec/` y abrí `index.html`. Es el menú: la barra queda a la
izquierda y la herramienta se abre embebida al lado.

Tiene que abrirse **desde adentro de su carpeta**: el menú busca `productos/` al lado suyo,
así que si se copia el `index.html` solo, las herramientas quedan en blanco.

Sin conexión andan igual. Lo único que necesita red es generar el Word o el PDF, y cuando
no puede, lo dice.

## Qué hay

| | |
|---|---|
| **Declaraciones juradas** | Convierte el formulario del declarante en el texto notarial ordenado. Es la más usada. |
| **Recibo de DDJJ** | El comprobante de la presentación. |
| **Primer testimonio** | Toma la escritura de protocolo en Word y la devuelve convertida en testimonio, con el folio, el cierre y las fojas. El cuerpo no se toca: misma fuente, mismos márgenes, mismo interlineado. |
| **Rogador** | Lee la escritura y completa el formulario de rogación del Registro. |
| **Escrituras sociales** | El circuito entero de un barrio: lotes, carátula, aviso de vencimiento del certificado, la minuta base del barrio y la minuta de cada lote. Prototipo. |
| **Expropiaciones** | Cuatro pasos, del expediente a los documentos. Prototipo. |
| **Buscador de modelos** | Para la cola larga de trámites raros, donde el problema no es llenar el modelo sino saber cuál de los veinte es el que va. En preparación. |

En `demos/` hay tres maquetas: son para mostrar, no para producir.

## Las dos ideas que sostienen todo

**La plantilla es un dato, no código.** Quien arma la minuta base pega una escritura ya
hecha, marca los tramos variables y exporta un `.json`. Cada auxiliar lo carga y completa
solo los datos de su lote. Si la minuta cambia, se cambia la base y los datos cargados no
se pierden. Es el diseño al que se está migrando el resto.

**Nada falla callado.** Si un dato no se pudo leer, la herramienta deja una nota en vez de
poner un blanco o de inventar. La revisión de la escribana es obligatoria y no se puede
ahorrar: la herramienta hace la escritura más fácil de confeccionar, no más confiable por
sí sola.

## Reglas notariales que el código respeta

No son preferencias de estilo: salieron de comparar escrituras reales.

- **Números en letras** —medidas, superficies, lotes, manzanas, vértices—, y los ángulos en
  numerales (78°27'). Las superficies totales en mayúscula.
- **Fechas siempre día, mes y año.** Se cargan con máscara `dd/mm/aaaa` y no con el
  selector del navegador, que muestra el mes primero según cómo esté configurada cada
  máquina. Una captura en una máquina en inglés mostró el mismo dato como `06/24/2025` y
  como `24/06/2025` en la misma pantalla: por eso la regla.
- **Apellido en mayúscula dentro del nombre en negrita**: `Juan Domingo PANIAGUA`.
- **Texto corrido**: un solo párrafo justificado, Cambria 12, interlineado 28,5 pt, A4 con
  márgenes de arriba 3,5 · abajo 0,6 · izquierda 5,3 · derecha 1,7 cm.
- **La mayúscula del apellido es del documento que se produce, no del que se lee.** Ninguna
  detección se apoya en el formato del texto de entrada.
- **Los nombres de las escribanas no están en el código.** El cargo sí, porque lo nombra la
  ley; el nombre es de quien ocupa el cargo y se carga una vez.

## Cómo está ordenado

Un producto, una carpeta, con su `NOTAS.md`: qué es, cómo guarda, sus reglas y lo que le
falta. El estado —en uso, prototipo, en preparación— no se expresa con la ubicación sino
con un dato, la lista `HERRAMIENTAS` de `sigec/index.html`. Sumar un producto es crear una
carpeta, no reacomodar el árbol.

```
sigec/
  index.html          el menú
  productos/
    ddjj/  recibo-ddjj/  primer-testimonio/  rogador/
    escrituras-sociales/  expropiaciones/  modelos/
demos/                maquetas para mostrar
```

## Qué no está en esta copia

- **La herramienta de un barrio concreto**, porque los transmitentes de ese barrio son
  parte de la plantilla —comparecen en todas sus escrituras— y no hay forma de
  anonimizarla sin romper el documento.
- **Una demo integrada que quedó vieja**, escrita cuando se pensaba que esto iba adentro
  del sistema oficial.
- El material de referencia: escrituras hechas, modelos provinciales y municipales,
  requisitos del Registro.
