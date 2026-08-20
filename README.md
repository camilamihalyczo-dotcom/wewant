# wewant — diseño y desarrollo web

Landing del servicio de desarrollo web de wewant (`[serv 04]`), con los cuatro niveles, las automatizaciones de IA y los mockups interactivos.

## Qué hay acá

Sitio estático, sin dependencias ni build. Todo el HTML, CSS y JavaScript viven en `index.html`.

```
index.html                       todo el sitio
assets/ww-mark-white.png         isotipo (blob) en blanco
assets/ww-wordmark-white.png     wordmark "wewant" en blanco
assets/favicon.png               favicon
```

Los assets salieron de `Documents/wewant`: el isotipo se derivó de `w-logo.png` (invertido a blanco con fondo transparente) y el wordmark es `logos_ww-02.png` recortado.

## Configuración

Arriba de todo del `<script>` está el bloque `CONFIG`:

```js
const CONFIG = {
  whatsapp : "5491112345678",   // número real, sin +, sin espacios ni guiones
  email    : "hola@wewant.ar",
  instagram: "wewant.creatives"
};
```

De ahí salen todos los botones de WhatsApp y el mail del pie. El botón de cada nivel manda el mensaje ya escrito con el nivel que la persona estaba mirando, así se sabe de dónde viene la consulta.

**Pendiente:** el número de WhatsApp es de ejemplo. Cambiarlo antes de publicar.

## Contenido editable

Todo el contenido son arrays de JavaScript. Se agregan o sacan elementos y la página se re-arma sola.

| Array | Qué controla |
|---|---|
| `NIVELES` | los cuatro niveles: nombre, descripción, qué es, qué incluye, para quién, rubros |
| `AUTOS` | las ocho automatizaciones de IA |
| `CLIENTES` | la grilla de clientes |

Los mockups interactivos viven en el objeto `DEMOS` (`m1` a `m4`). Cada uno arma su propio HTML: el teléfono con la landing, el selector de pedidos que compone el mensaje de WhatsApp, el panel editable con vista pública en vivo y el dashboard con pestañas.

## Sistema de diseño

Tomado del branding de wewant.

- **Fondo** negro puro `#000`, texto `#F0F0F0`, grises `#8C8C8C` y `#5A5A5A`
- **Tipografía** Poppins en pesos 200/300/400. Todos los títulos en minúscula, tracking negativo
- **Etiquetas** entre corchetes con numeración: `[ serv 04 ]`, `[ web 01 ]`, `[ ia 03 ]`
- **Grilla** líneas verticales finas fijas de fondo, más marcadores geométricos (rombo, círculo, triángulo) en contorno
- **Sin color.** La única excepción es el verde de WhatsApp dentro del mockup de pedidos, porque ahí representa la app real

Las variables CSS están al principio del `<style>`.

## Cómo verlo local

Los assets son rutas relativas, así que conviene servirlo en vez de abrir el archivo directo:

```bash
npx serve .
```

## Deploy

Vercel como sitio estático. Framework Preset en **Other**, sin build command ni output directory.

## Pendientes

- [ ] Número real de WhatsApp y mail en `CONFIG`
- [ ] Imagen de Open Graph 1200×630 para que el link se vea bien al compartirse
- [ ] Reemplazar los mockups ficticios por proyectos reales cuando estén
