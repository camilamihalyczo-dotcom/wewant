# Vitrina — Tu vidriera digital

Sitio de Vitrina: páginas web para negocios que quieren verse bien y vender sin complicaciones.

## Qué hay acá

Un sitio estático, sin dependencias ni build. Todo el HTML, CSS y JavaScript viven en `index.html`.

```
index.html     todo el sitio (estructura + estilos + interacciones)
favicon.svg    el toldo de Vitrina
```

## Cómo cambiar el WhatsApp

Abrí `index.html`, buscá el bloque `CONFIG` (arriba de todo del `<script>`) y cambiá los tres datos:

```js
const CONFIG = {
  whatsapp : "5491112345678",   // tu número, sin +, sin espacios ni guiones
  email    : "hola@vitrina.com.ar",
  instagram: "vitrina"
};
```

Todos los botones de "Pedir presupuesto" salen de ahí. El del panel de cada nivel manda el mensaje ya escrito con el nivel que el visitante estaba mirando.

## Cómo editar los niveles

En el mismo `<script>` está el array `NIVELES`. Cada nivel es un objeto con:

| Campo | Qué es |
|---|---|
| `nombre` | el título de la tarjeta |
| `bajada` | la línea de abajo del título |
| `que` | el párrafo "Qué es" |
| `incluye` | la lista con tildes |
| `para` | el párrafo "Para quién" |
| `rubros` | las etiquetas redondeadas |

Los add-ons están en `ADDONS` y las frases del traductor de jerga en `TRAD`. Se agregan o sacan elementos del array y la página se re-arma sola.

## Cómo verlo local

No hace falta servidor: doble clic en `index.html` y listo. Si preferís servirlo:

```bash
npx serve .
```

## Deploy

Está pensado para Vercel como sitio estático. No requiere framework, ni build command, ni output directory — Vercel sirve `index.html` desde la raíz.

## Pendientes

- [ ] Cargar el número real de WhatsApp en `CONFIG`
- [ ] Reemplazar los mockups ficticios por proyectos de clientes reales cuando los haya
- [ ] Sumar imagen de Open Graph (`og:image`) para que se vea bien al compartir
