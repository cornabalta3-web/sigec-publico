# Recibo DDJJ

Constancia de recepción de la declaración jurada patrimonial.

- **Estado:** en uso.
- **Archivo:** `recibo-ddjj.html`
- **En SIGEC:** `id:"recibo"`, grupo Declaraciones juradas, color `#1A2744` / `#8FA8D4`
  en oscuro. El azul casi negro desaparecía sobre fondo oscuro: por eso la variante.

## Cómo está armado

- El orden de los campos sigue al DNI: **Género → Nombres → Apellido → DNI → Formulario
  → N.º de escritura → Fecha**. Se carga leyendo el documento, no al revés.
- El apellido se pasa a mayúscula solo, con `toLocaleUpperCase("es-AR")`.
- La fecha viene puesta con la de hoy, y limpiar el formulario no la borra.
- El género **se deduce del nombre de pila** con las listas `EXC_F` (32 nombres) y
  `EXC_M` (11), que son las de `armador-minutas`, la versión más completa. La deducción
  **solo propone**: si la persona elige a mano (`generoElegido`), no se vuelve a tocar
  hasta limpiar los campos.
- La hoja es A4 (`aspect-ratio:1/1.414`) a 18px. El documento va en **Times New Roman**,
  no en la tipografía de la interfaz: es un documento, no una pantalla.

## No llama a la API

## Pendiente

- Hay que reformarlo (decisión 7): se usa mucho y todavía tiene la plantilla escrita en
  el código.

## Hecho el 15/09/2026

- **Los huecos se imprimían como si fueran datos.** En pantalla un campo vacío se ve gris
  e itálico, pero `@media print` lo forzaba a `color:#000` y `font-style:normal`, así que
  «(apellido)» salía en el papel con el mismo aspecto que un apellido de verdad. Ahora en
  el papel es lo que es —un renglón en blanco para completar a mano— y antes de imprimir
  se avisa cuántos huecos van.
- **`partesFecha` aceptaba fechas que no existen:** validaba mes 1-12 y día 1-31, sin días
  por mes ni bisiesto, así que tipeando 31/02/2026 la pista confirmaba «31 de febrero».
  Ahora valida los días de cada mes. Verificado: 29/02/2024 pasa, 29/02/2026 y 31/04/2026 no.
