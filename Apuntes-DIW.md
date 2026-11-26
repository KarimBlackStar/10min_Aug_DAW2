# Apuntes Progresivos

## HTML Semántico y Accesibilidad

- **`<hgroup>`:** Agrupa un encabezado (`h1`-`h6`) con su subtítulo o lema para indicar a los lectores de pantalla que forman una sola unidad de título.
- **`<header>` dentro de `<section>`:** No es exclusivo de la cabecera principal; sirve para introducir cualquier sección (etiqueta, título y descripción) separándola de su contenido.
- **Listas (`<ul>` / `<li>`) frente a `<div>`:** Usa `<ul>` y `<li>` para colecciones de elementos relacionados (como tarjetas o menús). Los hijos directos de un `<ul>` siempre deben ser `<li>`, nunca `<div>`.
- **Uso de `<div>`:** Reservado solo para agrupar elementos con fines puramente visuales (CSS) cuando no existe una etiqueta semántica adecuada.
- **Múltiples clases en un mismo elemento (`class="clase1 clase2"`):** En el atributo `class`, el espacio separa clases independientes (para nombres compuestos en una sola clase se usa guion, como `header-logo`). Esto permite combinar una clase común compartida (como `.panel` para fondo, sombra y padding) con una clase específica (`.introduccion`, `.cartel`) sin repetir código.

## CSS: Flexbox, Grid, Tipografía y Responsive

- **`display: flex` / `display: grid` se aplica al padre para controlar a sus hijos directos:** Propiedades como `justify-content`, `align-items`, `flex-wrap` o `gap` no funcionan si se ponen en elementos que no son contenedores flex/grid (por ejemplo, en `<img>`, `<li>`, `<a>`, `<figcaption>`, `<input id="privacidad">` o una `.card` sin `display: flex`). Deben ir en su contenedor padre directo.
- **Inversión de ejes en Flexbox y texto multilínea:** Con `flex-direction: row` (por defecto), `justify-content` centra en horizontal y `align-items` en vertical. Con `flex-direction: column`, los ejes se invierten y `align-items: center` centra las cajas en horizontal; pero si un `<p>` tiene varias líneas (como en `.pie`), necesitas también `text-align: center;` para centrar las líneas internas.
- **`flex-wrap: wrap`:** Permite que los elementos de un contenedor flex (como los enlaces de un menú en móvil) bajen automáticamente a una segunda línea cuando no caben.
- **`flex: 1` frente a `justify-content: space-between`:** `flex: 1` hace que un hijo flex absorba todo el espacio sobrante. Si quieres que `justify-content: space-between` empuje dos elementos a los extremos (como logo y título en escritorio), no pongas `flex: 1` en uno de ellos.
- **Cuadrícula responsive automática en Grid:** En `.cards` (`display: grid; gap: 1.5rem;`), añadir `grid-template-columns: repeat(auto-fit, minmax(280px, auto));` a partir de tableta (`@media (min-width: 768px)`) crea columnas automáticas según el espacio disponible.
- **Combinar ancho completo y 2 columnas en un mismo Grid (Escritorio):** Para poner dos secciones lado a lado (`.lendas` y `.contacto`) en `@media (min-width: 1024px)`, define `grid-template-columns: repeat(2, 1fr);` en el padre (`.contenido`) y haz que las secciones anteriores ocupen todo el ancho con `grid-column: 1 / -1;`. Para texto + bloque de fecha en `.introduccion`, usa `display: grid; grid-template-columns: 1fr auto; align-items: center;`.
- **Centrar una imagen directamente:** Una `<img>` es `inline` por defecto. Para centrarla sin Flexbox: `display: block; margin: 0 auto;` junto con un `width` menor al `100%`.
- **Centrar un contenedor principal con ancho máximo:** Combinar `width: min(1120px, 92%);` con `margin: 2rem auto;` centra el bloque (`.contenido`) dejando márgenes iguales a izquierda y derecha.
- **Relleno interno (`padding`) en el padre y relleno fluido:** El `padding` se aplica una sola vez al contenedor padre (`.panel`) para empujar hacia adentro a todos sus hijos a la vez. Con `padding: clamp(1.2rem, 3vw, 2rem);` se consigue un relleno fluido que crece y decrece de forma continua según el ancho de pantalla sin necesitar media queries.
- **Cajas cuadradas con `aspect-ratio: 1 / 1`:** Si combinas `aspect-ratio: 1 / 1` con `width: fit-content` y el contenido apilado es más alto que ancho, CSS estira la caja hacia abajo y rompe el cuadrado. Para evitarlo, dale un ancho suficiente (`width: 12rem;`) y ajusta el `line-height: 1;` y `margin: 0;` en los textos grandes internos.
- **Sombra de texto (`text-shadow`):** Sintaxis `eje-X eje-Y difuminado color` (ejemplo: `text-shadow: 2px 3px 8px #24130d;`).
- **Subrayado de enlaces y puntos de listas:** En enlaces (`<a>`) se quita con `text-decoration: none;`, y si necesitas devolverlo en uno concreto (como en `.pie a`), usa `text-decoration: underline; text-underline-offset: 4px;`. En listas (`<ul>`) se usa `list-style: none; padding: 0;` para quitar tanto las viñetas como el hueco izquierdo por defecto.
- **Filtros de imagen y transiciones en `:hover`:**
  - Efecto vintage que recupera color original (`.cartel-figura img`): `filter: sepia(40%) contrast(110%) brightness(95%); transition: filter 0.5s ease;` en reposo y `filter: none;` en `:hover`.
  - Tarjetas interactivas (`.card`): elevación vertical con `transform: translateY(-4px);` en `.card:hover` (`transition: transform 0.2s ease;`) e iluminación de imagen al pasar por cualquier punto de la tarjeta usando `.card img { height: 190px; object-fit: cover; filter: brightness(85%); transition: filter 0.2s ease; }` y `.card:hover img { filter: brightness(100%); }`.
- **Línea animada desde el centro en hover/focus (`::after` sin `position`):** En lugar de usar `position: relative/absolute` y `transform`, pon `display: inline-block;` en el enlace (`a`) y crea la línea en `a::after` con `content: ""; display: block; margin: 4px auto 0; width: 0; height: 3px; transition: all 0.5s ease-out;`. Al pasar a `width: 100%` en `:hover::after` y `:focus-visible::after`, el `margin: auto` la mantiene centrada y la hace crecer desde el centro hacia ambos lados.
- **Hover solo en dispositivos con ratón:** Para evitar el *sticky hover* en móviles táctiles, se puede envolver el `:hover` en `@media (hover: hover) { ... }` dejando `:focus-visible` fuera para mantener la accesibilidad con teclado.
- **Selectores básicos y errores típicos:** `#id` apunta a un atributo `id`, `.clase` apunta a una `class` (olvidar el punto, como escribir `btn:focus-visible` en vez de `.btn:focus-visible`, rompe la regla porque busca una etiqueta `<btn>`), y `>` selecciona solo hijos directos. Evita también dejar reglas duplicadas en el archivo.
- **Tipografía fluida con `clamp(min, ideal, max)`:** Ajusta el tamaño automáticamente según el ancho de pantalla sin necesidad de media queries. Es obligatorio dejar espacios alrededor de `+` o `-` dentro de la fórmula (ejemplo: `clamp(2.2rem, 1.5rem + 4vw, 4.5rem)`).
- **Herencia de `font-size`:** Un `font-size` en un contenedor padre (como `<hgroup>`) no cambia el tamaño de un `<h1>` si el `<h1>` ya tiene su propio tamaño definido.
- **Mobile First y `@media`:** Se diseñan primero los estilos de móvil fuera de media queries y al final del CSS se añaden los cambios para pantallas mayores (por ejemplo, `@media (min-width: 768px)` para tableta y `@media (min-width: 1024px)` para escritorio).

## CSS: Formularios y Botones

- **Estilos en los campos, no en `<label>`:** Las reglas de `width: 100%`, `padding: 0.75rem`, `border-radius` y `border: 1px solid ...` van en `input:not([type="checkbox"]), select, textarea`. Poner un `height` pequeño en `<label>` aplasta su caja y rompe el centrado con Flexbox.
- **Estructura de campos con `<p>`:** Cada `<p>` del formulario agrupa `<label>` + campo en columna (`display: flex; flex-direction: column; gap: 0.5rem;`). Para el checkbox en horizontal, se sobreescribe en su `<p>` padre (`.contacto form .aceptacion { flex-direction: row; align-items: center; }`).
- **Accesibilidad visual con `:has(:focus)`:** Permite destacar el `<label>` cuando su campo asociado recibe el foco sin usar JavaScript: `.contacto form p:has(:focus) label { font-weight: 700; }`.
- **Personalizar checkbox:** Escalarlo con `transform: scale(1.2);` y cambiar su color de marca con `accent-color: var(--color-secundario);`.
- **Botones (`.btn`) accesibles:** Quitar el borde gris nativo con `border: none;`, añadir `cursor: pointer;`, y aplicar el mismo contorno en `:hover` y `:focus-visible` con `outline: 3px solid var(--color-secundario); outline-offset: 4px;` (`gap` no funciona en botones, solo en contenedores flex/grid).

## Depuración con DevTools (F12 en Chrome)

- **Comprobar Flexbox y Grid:** En la pestaña *Elements*, haz clic en la etiqueta `flex` o `grid` junto al contenedor padre para ver las líneas guía y columnas pintadas en pantalla.
- **Comprobar fuentes y `clamp()`:** Inspecciona el elemento exacto (por ejemplo `<h1>`, no `<body>`), ve a *Computed* y revisa `font-size` al cambiar el ancho en modo responsive (`Ctrl + Shift + M`), y baja hasta *Rendered Fonts* para confirmar que carga la fuente local.
- **Probar `@media` en vivo y detectar por qué no saltan:** En el modo responsive (`Ctrl + Shift + M`), escribe el ancho exacto manualmente (ej. `768` o `1200`), porque tener DevTools acoplado a la derecha reduce el ancho real del viewport. Si la regla `@media` ni siquiera aparece en *Styles*, revisa si falta una llave de cierre `}` justo encima en tu CSS.
- **Congelar `:hover` para depurar `::after` o `filter`:** En la pestaña *Styles*, pulsa el botón `:hov` y marca `:hover` para dejar el estado fijo y poder inspeccionar el pseudoelemento `::after` o alternar el `filter: none;` en tiempo real.
- **Probar clases independientes (`.cls`):** En la pestaña *Styles*, pulsa el botón `.cls` para ver cada clase del elemento como una casilla separada y poder activarlas o desactivarlas en vivo.
- **Detectar de dónde sale el aire extra en una caja:** Al pasar el ratón por un elemento en *Elements*, las franjas naranjas son `margin`, las verdes son `padding` y una zona azul muy alta alrededor de un número o texto indica un `line-height` alto.
