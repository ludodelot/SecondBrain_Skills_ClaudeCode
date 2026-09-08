---
type: skill-note
skill: "Claude Code — deep research: cómo hacer mejores webs"
date: 2026-09-08
tags: [claude-code, web-design, deep-research, component-libraries, design-trends]
---

# Deep Research: cómo hacer webs mucho mejores con Claude Code (2026-09-08)

Contexto: [[README|Claude Code — README]]. Segunda vuelta de investigación después de que el primer rediseño de [ludodelot.github.io](https://github.com/ludodelot/ludodelot.github.io) (Liquid Glass v1) no convenció — objetivo: profundizar mucho más en **proceso**, **librerías/repos** y **tendencias de diseño 2026**, no solo en la técnica de un efecto visual puntual. Complementa [[Web_Design_Skills_Catalogo|catálogo de skills anti-AI-slop / Apple HIG]].

## 1. El workflow real de "Claude Code para diseñadores" en 2026

Lo que separa un resultado mediocre de uno bueno no es el modelo, es el **proceso**:

1. **No saltarse la preparación.** El error más común es pedir "hazme una web bonita" directo — sin definir sujeto, audiencia, tono, referencias. Esto es exactamente lo que ya cubre la skill `frontend-design` (ver [[Web_Design_Skills_Catalogo|catálogo]] §2): plan de diseño (paleta nombrada, tipografía con roles, wireframe ASCII, principios) **antes** de escribir código, y revisar ese plan contra los defaults genéricos.
2. **Iteración en tiempo real con dev server local**, no solo generar y mirar el archivo estático — correr un server local (`python3 -m http.server` para HTML plano, o `npm run dev` en un stack con build) y pedir cambios puntuales en lenguaje natural mientras se ve el resultado. Para este proyecto (HTML estático) esto es trivial y se debería hacer *antes* de dar por bueno un rediseño.
3. **Screenshot loop**: pedirle al agente que tome captura de su propio resultado (headless browser / Puppeteer / Playwright) y se autocritique contra el brief — el proceso de `frontend-design` lo pide explícitamente ("a picture is worth 1000 tokens"). Esto se puede automatizar con la skill `browse` de gstack si está disponible, o con un script simple de Playwright.
4. **Stacking de skills**: combinar `frontend-design` (dirección estética) + `hallmark` (auditoría anti-slop) + una skill de research web (`Firecrawl`/`defuddle`) cuando se necesita estudiar referencias reales, en vez de improvisar sobre una sola pasada.
5. **Integración con Figma vía MCP** (si en algún momento se diseña primero en Figma): un MCP bidireccional que trae tokens de diseño, spacing, y variables de color directo a Claude Code — relevante si en el futuro se diseña la web en Figma antes de codearla, no aplica todavía a este proyecto que es directo a código.

**Lección clave para nuestro caso:** el rediseño "Liquid Glass v1" se hizo en una sola pasada sin loop de captura/crítica visual — probablemente por eso no convenció. La corrección de proceso: (a) tomar referencias reales concretas primero (sección 4 abajo), (b) iterar con captura de pantalla antes de dar por terminado, (c) no comprometerse con un único efecto visual (glass) como la idea central sin validar que encaja con el contenido (un CV profesional dual BI+fotografía puede pedir algo distinto a un efecto de vidrio).

## 2. Librerías/componentes de UI mejor valorados en 2026 (para tener en el radar, no necesariamente instalar todas)

| Librería | Stars aprox. | Para qué sirve | Cuándo usarla |
| --- | --- | --- | --- |
| [shadcn/ui](https://ui.shadcn.com) | ~114k★ | La librería de componentes React más adoptada — no es una dependencia de npm sino código que se copia a tu repo (control total). Respaldada por Vercel, integración nativa con Next.js. | Base de cualquier proyecto React/Next nuevo — formularios, tablas, nav. |
| [Magic UI](https://magicui.design) | ~21k★ | Capa de animación sobre el ecosistema shadcn — 150+ componentes animados (React + TS + Tailwind + Motion). | Hero sections y landing pages que necesitan movimiento sin reinventar la rueda. |
| [Aceternity UI](https://ui.aceternity.com) | ~28k★ | Efectos "premium" con Framer Motion — 200+ componentes, se actualiza mensualmente. | Cuando una sección necesita sentirse "diseñada por un equipo in-house", no solo funcional. |
| Motion Primitives / Cult UI | — | Componentes animados construidos específicamente sobre Motion (ex-Framer Motion). | Alternativas más ligeras a Aceternity cuando se quiere menos "premium/denso". |

**Patrón práctico recomendado por la industria en 2026:** usar shadcn como fuente de verdad de bloques → capa de animación con Magic UI/Motion Primitives → Aceternity solo para 1-2 secciones que deban destacar. **Nota importante para este proyecto:** nuestro sitio es HTML/CSS estático sin build tool (sin React) — adoptar shadcn/Magic UI/Aceternity tal cual implicaría migrar a un stack con Node/React, lo cual es una decisión de arquitectura grande. Alternativa sin migrar: tomar prestados patrones visuales concretos (bento grid, tipografía, micro-interacciones) e implementarlos a mano en CSS/JS vanilla, que es lo que ya se ha estado haciendo.

## 3. Librerías de animación (más allá de CSS puro)

| Librería | Rol | Notas |
| --- | --- | --- |
| **Motion** (antes Framer Motion) | Animación declarativa para React (ahora también Vue/vanilla desde 2025). | La opción "todo terreno" según LogRocket 2026 — buen balance potencia/facilidad/rendimiento. |
| **GSAP** | Animación basada en timeline, scroll-driven, morphing SVG. | La mejor opción cuando se necesita control fino de secuencias o "scrollytelling" (ver §4). Ya identificada como opción en [[Web_Design_Skills_Catalogo|catálogo]] §4. |
| **AOS (Animate On Scroll)** | Scroll reveals "zero-config". | Equivalente ligero a lo que ya implementamos a mano con `IntersectionObserver` — no hace falta añadir la dependencia, nuestro código vanilla ya cubre ese caso. |
| **Anime.js** | Tweens pequeños y puntuales. | Útil para un solo micro-momento (ej. el ícono del toggle de tema), no para orquestar toda la página. |

**Para un sitio estático sin build tool:** no vale la pena cargar una librería completa por 1-2 animaciones — seguir con CSS + `IntersectionObserver` vanilla es lo correcto salvo que se decida migrar a scrollytelling real (ver abajo), donde GSAP sí justificaría su peso.

## 4. Tendencias de diseño 2026 relevantes para un portfolio/CV personal

- **Bento grid**: layouts tipo "caja bento" — contenido organizado en compartimentos de tamaños variados dentro de una grilla, en vez de columnas uniformes. Es la tendencia dominante de 2026 (se reporta +23% de scroll depth vs. grids de 12 columnas tradicionales en trabajos de agencia). Para un CV: encaja muy bien para la sección de **stats + skills + proyectos** — en vez de 4 stat-cards idénticas en fila, un bento con celdas de distinto tamaño (una grande con la foto/highlight, celdas medianas con métricas, una celda ancha con la línea de tiempo resumida).
- **Scrollytelling**: contar una historia mientras se hace scroll — el fondo cambia, una animación se dispara, un gráfico se dibuja solo. Muy usado en portfolios premiados (Awwwards). Para nuestro caso: podría verse en la transición entre el track "BI/Data" y el track "Creative/Fotografía" — un cambio de paleta/fondo real al cruzar esa frontera, no solo un cambio de color de texto.
- **Sitios de referencia 2026** (Awwwards / Web Design Awards, categoría portfolio): Mitchell Hou (Site of the Day, jul 2026), "Portfolio as conversation", Yazdani Studio — patrón común: **una idea central fuerte por sitio** (no varias técnicas mezcladas), tipografía como protagonista, mucho espacio negativo, fotografía real a tamaño grande (no thumbnails).
- **Plantillas v0 de Vercel** (React/Next, útiles como referencia de composición aunque no se usen directo): plantillas de portfolio pensadas para "case studies inmersivos" — la estructura es: hero con una sola pieza de trabajo destacada → grid de proyectos → sección "sobre mí" al final, no al principio. Es una estructura invertida respecto a la nuestra (nosotros ponemos "About" primero) — vale la pena considerar si el objetivo es que el trabajo hable antes que la biografía.

## 5. Conclusión operativa para el próximo intento de rediseño

En vez de comprometerse de entrada con un solo efecto visual (glass, bento, scrollytelling), el siguiente rediseño debería:
1. Definir primero la **idea central única** del sitio (¿qué hace única a esta persona? — el cruce BI/data ↔ fotografía de festivales es en sí mismo el concepto distintivo, poco común, y debería ser el eje visual, no un efecto de moda).
2. Usar **bento grid** para la sección de stats/skills en vez de una fila pareja de tarjetas.
3. Reservar movimiento tipo scrollytelling para **un solo momento** (ej. el cruce entre los dos tracks profesionales).
4. Iterar con **capturas de pantalla reales** antes de dar el rediseño por terminado (loop de crítica, no una sola pasada).
5. Pedir al usuario fotos reales de festivales/Delot Media antes de comprometer el layout de la sección creativa — con placeholders/orbes decorativos se pierde el argumento visual más fuerte que tiene este perfil en particular.

## Fuentes
- [Untitled UI — React component libraries 2026](https://www.untitledui.com/blog/react-component-libraries)
- [Aceternity — Best React UI components 2026](https://ui.aceternity.com/guides/best-react-ui-components-2026)
- [PkgPulse — Aceternity vs Magic UI vs shadcn 2026](https://www.pkgpulse.com/guides/aceternity-ui-vs-magic-ui-vs-shadcn-animated-react-2026)
- [LogRocket — Best React animation libraries 2026](https://blog.logrocket.com/best-react-animation-libraries/)
- [925 Studios — Claude Code for Designers 2026](https://www.925studios.co/blog/2026-03-18-claude-code-for-designers-the-complete-guide-to-building-real-websites-in-2026)
- [Superfiles — Bento Grid UI Design Guide 2026](https://superfiles.in/bento-grid-ui-design-trend.php)
- [WriterDock — Bento Grids & Beyond, UI trends 2026](https://writerdock.in/blog/bento-grids-and-beyond-7-ui-trends-dominating-web-design-2026)
- [Web Design Awards — Portfolio winners 2026](https://www.webdesignawards.io/winners/2026/portfolio)
- [Vercel v0 — Portfolio templates](https://v0.app/templates/blog-and-portfolio)
