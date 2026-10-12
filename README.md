# Coffea — Landing page (HTML + CSS)

Landing estática de una página para la cafetería ficticia **Coffea**, según el PRD de Diseño UX-UI 2026 (29/09/2026).
Solo HTML5 semántico y CSS3: sin JavaScript, frameworks ni preprocesadores.

## Estructura

```
coffea-landing/
├── index.html
├── 404.html
├── netlify.toml
├── css/
│   ├── main.css          # @import de las capas (solo desarrollo)
│   ├── tokens.css        # declara @layer + custom properties (único lugar con valores crudos)
│   ├── base.css          # reset, tipografía base, :focus-visible
│   ├── layout.css        # .container, .section-title, .btn
│   ├── components/       # header, hero, categories, product-card, carousel,
│   │                     # promo, testimonial-card, newsletter, footer
│   └── utilities.css     # .visually-hidden, .skip-link, prefers-reduced-motion
└── assets/
    ├── img/              # imágenes (hoy son placeholders SVG, ver "Assets")
    └── icons/            # SVG de íconos
```

## Convenciones

- BEM (`.product-card`, `.product-card__title`, `.testimonials__item--featured`).
- Todo color, fuente, radio y espaciado sale de `css/tokens.css`.
- Mobile-first: estilos base para 360 px y `@media (min-width: …)` hacia arriba (768 / 1024 / 1440).
- Capas: `@layer tokens, base, layout, components, utilities`.
- `position: absolute` solo para superposiciones (header sobre hero, corazón sobre imagen, imágenes decorativas).
- `index.html` enlaza cada CSS con `<link>` directo (sin cadena de `@import`). Los links llevan `?v=1`.

## Levantar en local

Cualquier servidor estático sirve (no hace falta build):

```bash
python3 -m http.server 8000
# abrir http://localhost:8000
```

## Despliegue en Netlify

1. Subir el proyecto a un repo de GitHub/GitLab.
2. En Netlify: *Add new site → Import an existing project* y elegir el repo.
3. Build command: vacío. Publish directory: `.` (ya está en `netlify.toml`).
4. La rama `main` publica a producción; cada Pull/Merge Request genera un Deploy Preview.
5. HTTPS y redirección HTTP→HTTPS los gestiona Netlify. `404.html` se sirve automáticamente.
6. Medir Lighthouse sobre la **URL de producción** de Netlify, no en local.

**Caché `immutable`:** `css/` y `assets/` se cachean un año. Si cambiás un archivo, subí el número de `?v=` en
`index.html` y `404.html` (o renombralo, p. ej. `main.v2.css`) para que el navegador lo vuelva a descargar.

## Assets

Ya son **reales** (exportados del Figma, optimizados a AVIF/WebP + JPG con `srcset`):
hero, 8 fotos de producto (4 de café + 4 de la sección dessert), mano con granos, granos del CTA,
granos del newsletter, avatar, íconos de categorías (vectorizados desde el PNG), corazón (la variante rellena se derivó del contorno) y las 4 redes sociales.

Siguen siendo **placeholders** (dibujados a mano, reemplazar por los exportados en SVG):
`search.svg`, `arrow.svg`, `chevron.svg`, `mail.svg`.

Notas:
- Las 4 fotos de "Our Special Dessert" son fotos de café, igual que en el Figma. Reemplazar por postres cuando existan
  (`dessert-1…4-334/668.{avif,webp,jpg}`) y ajustar los `alt`.
- Un solo avatar (`avatar-1`) se reutiliza en las 3 cards, como en el diseño.
- Los íconos de categorías están inline en `index.html` para heredar `currentColor` (hover a `--color-espresso`).
- Colores extraídos del Figma: estrellas `#FFB921` y vacías `#CECECE`.

## Decisiones pendientes (riesgos del PRD)

| Tema | Estado actual |
|---|---|
| Diseño tablet/mobile | Implementada la propuesta de la sección 7; validar con diseño en Fase 3 |
| Typos del Figma | Corregidos en el HTML (“get”, “Our story”, “INFORMATION”, “Plans & pricing”, “products”, “newsletter and get”) |
| Marca | “Coffea” en header y footer |
| Contenido placeholder | Nombres de productos inventados, precio `Rs. 200`, testimonios “James Smith” + lorem ipsum (copy del hero, newsletter y footer tomado del Figma) |
| Moneda | Se mantiene “Rs.”; confirmar si pasa a $ (ARS) |
| Sección dessert | Un solo título; fotos de postre ilustrativas hasta tener las reales |
| Carrusel sin JS | 4 cards por carrusel: en desktop se ven completas y las flechas son solo decorativas; en mobile/tablet se desliza con swipe y `scroll-snap` |
| Idioma | `lang="en"` (copy del diseño); etiquetas accesibles en español según el PRD |

## Definition of Done (estado)

- [x] 9 secciones implementadas (alturas a 1920 px: hero 1000, categorías 220, productos 742, CTA 335, testimonios 471, newsletter 320, footer 358, card 570)
- [x] Sin JavaScript
- [x] Sin scroll horizontal entre 360 y 2560 px (verificado en Chromium)
- [ ] 0 errores en validadores HTML/CSS del W3C
- [ ] Lighthouse ≥ 90 Performance y ≥ 95 Accessibility / Best Practices / SEO (en producción)
- [ ] Probado en Firefox, Safari (macOS/iOS) y Samsung Internet; dispositivo real iOS y Android
- [ ] Comparación lado a lado con el Figma (≤ 4 px)
- [ ] Sitio publicado en Netlify con deploy continuo, HTTPS y Deploy Previews
