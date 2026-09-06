# wewant — diseño y desarrollo web

Landing del servicio de desarrollo web de wewant (`serv 04`), armada como una revista: once spreads a pantalla completa que se pasan con el scroll.

## Qué hay acá

Sitio estático, sin dependencias ni build. Todo el HTML, CSS y JavaScript viven en `index.html`.

```
index.html                     todo el sitio
assets/w-glass.webp            la W cromática, recortada con transparencia
assets/caustics.webp           la textura de agua que rellena las formas y la W
assets/buenos-aires.webp       el Obelisco y la 9 de Julio, fondo del spread de contacto
assets/ww-wordmark-black.png   wordmark negro (nav)
assets/ww-wordmark-white.png   wordmark blanco
assets/ww-mark-black.png       isotipo negro
assets/favicon.png
```

Los assets salieron de `Documents/wewant`:

- **W cromática** — `Elements - wewant/wewant_glass.png`, con el fondo recortado por flood fill para que pueda ir sobre cualquier color
- **Textura de agua** — recortada de adentro del semicírculo de `wewant-content-01.png`, a resolución nativa. Es la misma que usan en las piezas de servicios
- **Obelisco** — franja izquierda del cover del Behance (`Branding/wewant_behance-01.png`), recortada para dejar afuera el logo y el wordmark que vienen incrustados en el centro de esa imagen

Todo pesa 448 KB.

## Los once spreads

| # | Spread | Fondo |
|---|---|---|
| 01 | portada — la W cromática | papel |
| 02 | manifiesto — la W con textura | negro |
| 03 | los cuatro servicios | papel |
| 04 | índice de niveles | hueso |
| 05 | web 01 — landing | negro |
| 06 | web 02 — pedidos | aqua |
| 07 | web 03 — editable | navy |
| 08 | web 04 — plataforma | papel |
| 09 | automatizaciones | negro |
| 10 | clientes | papel |
| 11 | contacto | el Obelisco, tipografía negra |

Cada nivel tiene su propia página con el mockup interactivo en un inset blanco al costado, como una foto en una revista.

## Configuración

Arriba de todo del primer `<script>`:

```js
const CONFIG = {
  whatsapp : "5491124034669",   // 11 2403-4669 con código de país y el 9 de celular
  email    : "wewantstudio1@gmail.com",
  instagram: "wewant.creatives"
};
```

De ahí salen todos los botones de WhatsApp y el mail del pie. El botón de cada nivel manda el mensaje ya escrito con el nivel que la persona estaba mirando, así se sabe de dónde viene la consulta.

Los datos ya son los reales.

## Contenido editable

Todo el contenido son arrays de JavaScript. Se agregan o sacan elementos y la página se re-arma sola.

| Array | Qué controla |
|---|---|
| `NIVELES` | los cuatro niveles: nombre, bajada, qué incluye, tema de color, folio |
| `AUTOS` | las ocho automatizaciones |
| `CLIENTES` | la grilla de clientes |

Para cambiarle el color a un nivel, se edita su campo `tema` con una de las clases de spread: `sp-papel`, `sp-hueso`, `sp-aqua`, `sp-azul`, `sp-navy`, `sp-tinta`. Si el fondo es oscuro, hay que sumar la clase al array `OSCUROS` para que el nav se aclare.

Los mockups viven en el objeto `DEMOS` (`m1` a `m4`): el teléfono con la landing, el selector que compone el mensaje de WhatsApp, el panel editable con vista pública en vivo y el dashboard con pestañas.

## Sistema de diseño

- **Base clara.** Hueso `#F3F1EC` y papel `#FBFAF7` con tinta `#0C0C0C`
- **Blanco y negro, y nada más.** El color entra únicamente por las texturas: el agua de las formas geométricas, la W cromática y la foto del Obelisco. Los únicos tres fondos que no son blanco ni negro están muestreados de esa misma agua:

  | Color | Hex | De dónde sale |
  |---|---|---|
  | azul petróleo | `#0F729F` | el medio tono del agua |
  | navy | `#003E64` | los fondos profundos de esa agua |
  | aqua | `#8BCFDC` | los brillos claros |

- **El motivo de forma con textura.** Círculos, triángulos y medias lunas rellenos con la textura de agua, recortados con `clip-path`. Es el mismo recurso de las piezas de servicios de Instagram
- **La W rellena de textura** (spread 02) no es una imagen nueva: se enmascara `caustics.webp` con la silueta de `ww-mark-black.png` usando `mask-image`, así sale idéntica a la pieza y no suma peso
- **Dos tipografías.** Anton en mayúscula para los titulares —el registro de los posts de Instagram— y Poppins en pesos 200/300 para el resto, que es el registro del deck. El interlineado de Anton está en 1.1: en esa fuente el acento de Á/É sube hasta ~0.95em y los descendentes bajan ~0.18em, así que con menos de 1.1 los acentos chocan con la línea de arriba
- **Folios.** Cada spread numerado abajo a la izquierda y con su corrida abajo a la derecha, como una revista. La corrida nunca repite la etiqueta de arriba, salvo en los spreads de nivel donde la repetición orienta
- **El nav se invierte solo** según el spread que esté en pantalla
- **Un solo color ajeno a la paleta:** el verde de WhatsApp dentro del mockup de pedidos, porque ahí representa la app real

## Cómo se pasa de página

En desktop (desde 900 px de ancho y 660 px de alto) el contenedor `.revista` usa `scroll-snap-type: y mandatory` y cada spread engancha al llegar. Las flechas y Re Pág / Av Pág también pasan de página. Debajo de esa medida el snap se desactiva y el scroll es normal, para que en el celular nada quede cortado.

## Recursos de animación

Todos disparan con un IntersectionObserver cuando el spread entra en pantalla, y respetan `prefers-reduced-motion`.

| Recurso | Dónde | Qué hace |
|---|---|---|
| `.pal` | titulares con `data-stagger` | el titular entra palabra por palabra, 55 ms entre cada una |
| `.tirante` | titulares con `.con-tirante` | la regla que sale de la última letra y se estira hasta el borde, como en "CONTENT DESIGN." |
| `.marquesina` | spread 03 | la frase de marca desfilando en loop |
| `.sello` | portada | el sello circular girando |
| `.w-glass` | portada | la W flotando |
| `.anim` | el resto | entrada escalonada de abajo hacia arriba |

Para sumar un tirante a un titular:

```html
<h2 class="display con-tirante a-sangre"><span data-stagger>Texto</span><i class="tirante"></i></h2>
```

`a-sangre` hace que la regla llegue al borde real de la pantalla; sin ella llega al borde de la columna.

## Cómo verlo local

Los assets son rutas relativas, así que conviene servirlo:

```bash
npx serve .
```

## Deploy

Vercel como sitio estático. Framework Preset en **Other**, sin build command ni output directory.

## Pendientes

- [ ] Imagen de Open Graph 1200×630 para que el link se vea bien al compartirse
- [ ] Reemplazar los mockups ficticios por proyectos reales cuando estén
