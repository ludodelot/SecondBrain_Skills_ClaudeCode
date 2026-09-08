---
type: skill-note
skill: "Claude Code — iconos, fondos animados y carruseles sin librerías"
date: 2026-09-08
tags: [claude-code, web-design, icons, animation, lucide]
---

# Investigación: iconos minimalistas, fondo aurora animado y marquee (2026-09-08)

Contexto: [[Deep_Research_Mejores_Webs_2026|deep research anterior]] — tercera vuelta de mejoras al CV, esta vez enfocada en quitar **todos los emojis** del sitio (petición explícita del usuario: "NUNCA EMOJIS, siempre iconos super bien hechos minimalistas elegantes estilo apple") y sumar movimiento de fondo + carrusel de skills.

## 1. Set de iconos: Lucide gana sobre Phosphor para estilo Apple

- **Lucide** (fork de Feather, 1500+ iconos): un solo estilo outline muy consistente — trazos limpios, uniones redondeadas, construcción geométrica. Al usar muchos iconos juntos, se ven como una sola familia. Es 16x más popular que Phosphor.
- **Phosphor** (1200+ iconos, 6 pesos: Thin/Light/Regular/Bold/Fill/Duotone): más flexible pero más peso por icono (carga los 6 pesos si no se hace tree-shaking a nivel de peso).
- **Conclusión para sitios minimalistas estilo Apple: Lucide** — consistencia + bundle más pequeño.
- Fuente: [PkgPulse — Lucide vs Heroicons vs Phosphor 2026](https://www.pkgpulse.com/guides/lucide-vs-heroicons-vs-phosphor-react-icon-libraries-2026), [wmtips comparación de mercado](https://www.wmtips.com/technologies/compare/lucide-vs-phosphor-icons/)

**Implementación real usada** (sitio HTML estático, sin build tool, sin permitir requests externos de más CDNs de los necesarios): en vez de cargar la librería completa de Lucide vía CDN, se construyó un **sprite SVG inline** (`<svg style="display:none"><symbol id="i-nombre">...</symbol></svg>`) con ~15 iconos hechos a mano en el mismo estilo (stroke uniforme, `stroke-width:1.7`, `stroke-linecap/linejoin: round`) y se referencian con `<svg class="icon"><use href="#i-nombre"/></svg>`. Ventajas: cero requests de red, cero dependencia externa, mismo control de color que el texto (`stroke: currentColor`).

## 2. Fondo "aurora" animado (tendencia 2026 en SaaS/portfolios)

- Técnica: 2-3 elementos `position:absolute` con `radial-gradient`, `filter: blur(60-80px)`, animados con `@keyframes` que mueven `transform: translate()/scale()` muy lentamente (25-40s por ciclo, `ease-in-out infinite alternate`).
- Es la versión "de bajo costo" (CSS puro, 60fps) de un mesh gradient real — no requiere WebGL ni librerías.
- Regla de accesibilidad: envolver siempre en `@media (prefers-reduced-motion: reduce) { animation: none !important; }`.
- Fuentes: [Superdesign — Aurora UI recipe](https://superdesign.dev/styles/aurora), [LunarLogic/auroral (CSS puro, GitHub)](https://github.com/LunarLogic/auroral), [Effect Labs — Animated backgrounds tutorial](https://effect-labs.com/en/pages/blog/backgrounds-animes-css.html)

## 3. Marquee/carrusel infinito sin librería

- Patrón estándar: contenedor con `overflow:hidden` + `mask-image: linear-gradient(90deg, transparent, #000 8%, #000 92%, transparent)` (desvanece los bordes) + un track interno duplicado **una vez** (el mismo contenido dos veces seguidas) animado con `@keyframes { to { transform: translateX(-50%); } }` en loop lineal infinito — al llegar a -50% se ve idéntico al inicio, dando un loop perfecto sin salto.
- `animation-play-state: paused` en `:hover` para que el usuario pueda leer el contenido si se detiene con el mouse.
- No hace falta ninguna librería (ni Swiper ni Marquee.js) para este caso de uso simple de una fila de chips/pills.

## Aplicado en
- [ludodelot.github.io](https://github.com/ludodelot/ludodelot.github.io) — commit `0067bf0`: sprite de iconos reemplazando cada emoji (🌙☀️📍🇲🇽🇫🇷), fondo aurora de 3 blobs, marquee de skills en la sección bento, además de nuevas tarjetas bento (educación con Minor en AI, años de experiencia, huella internacional, empresas) y un switch "Data & BI ⇄ Creative & Marketing" que reordena las dos líneas de tiempo de experiencia vía `order` de CSS.
