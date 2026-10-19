# SIGEC

Herramientas de trabajo de la Escribanía General de Gobierno. Son archivos HTML autónomos:
no hay servidor. Lo que se carga queda guardado **en el navegador de quien lo usa** y se
exporta a `.docx`, `.pdf` o `.json`.

**Para usarlas desde afuera de la oficina:** https://cornabalta3-web.github.io/sigec-publico/

## Qué hay acá

Sólo las herramientas en uso que no traen datos de personas reales:

| | |
|---|---|
| **Declaraciones juradas** | Convierte el formulario del declarante en el texto notarial ordenado. |
| **Recibo de DDJJ** | El comprobante de la presentación. |
| **Primer testimonio** | Toma la escritura de protocolo en Word y la devuelve convertida en testimonio. |
| **Rogador** | Lee la escritura y completa el formulario de rogación del Registro. |

Los nombres, documentos y números que aparecen en los ejemplos son inventados.

## Lo que hay que saber

- **Los datos no viajan.** Lo que se carga en una computadora queda en ese navegador: lo
  hecho desde afuera no aparece en la oficina, ni al revés. La clave de la API se carga de
  nuevo en cada navegador.
- **No está el turnero**, que guarda en una carpeta del disco de la oficina, ni las
  escrituras sociales.
- La copia de trabajo vive en el disco de red de la oficina. Esta se arma desde ella, sacando
  todo lo que no se puede publicar, y se actualiza cuando se actualiza aquélla.
