---
type: skill-note
skill: "Claude Code — skills de diseño web"
date: 2026-09-08
tags: [claude-code, web-design, ai-slop, apple-hig, animation, skills]
---

# Catálogo de skills de Claude Code para mejorar diseño web (investigación 2026-09-08)

Motivación: [[README|Claude Code — README]] — el pedido concreto fue mejorar [ludodelot.github.io](https://github.com/ludodelot/ludodelot.github.io) "muchísimo" y evitar que se vea genérico/hecho por IA. Ver también [[04_Knowledge/Skills/GitHub/README|Skills/GitHub]] para buenas prácticas de README/repo que complementan esto.

## 1. El problema: "AI slop" en diseño

Los modelos de IA, sin dirección, convergen en un puñado de defaults reconocibles:
1. Fondo crema cálido (~`#F4F1EA`) + serif display + acento terracota (~`#D97757`, el acento de Claude — se nota que es IA).
2. Fondo casi negro + un solo acento verde-ácido o bermellón brillante.
3. Layout "broadsheet": hairlines, cero border-radius, columnas densas tipo periódico.
4. "SaaS card kit": todo en cards redondeadas idénticas, mismo `border-radius`, misma sombra gris suave, gradientes decorativos.
5. Chrome de plantilla: eyebrow label en ALL-CAPS tracked-out sobre cada heading, metadatos unidos con `·`, labels tipo "WORD — fragmento" con em dash, negro-no-negro (`#0B0B0B`), monospace para datos pequeños, flecha `→` al final de todo link/botón.

Ninguno es malo per se — el problema es usarlos **sin que el brief los pida**, por default.

## 2. Skills anti-AI-slop

| Skill | Autor | Qué hace | Instalación |
| --- | --- | --- | --- |
| **frontend-design** | [anthropics/skills](https://github.com/anthropics/skills/blob/main/frontend-design/SKILL.md) (oficial) | La skill de diseño más instalada (855k+ installs). Fuerza un proceso de 2 pasadas: (1) plan de diseño — paleta de 4-6 hex nombrados, tipografía y sus roles, layout con wireframe ASCII, principios de qué la hace única — (2) revisar ese plan contra los 5 defaults de arriba y corregir antes de escribir código. Incluye guía de copywriting (voz activa, sin "Submit" genérico, sin eyebrow labels innecesarios). | `npx skills add https://github.com/anthropics/skills --skill frontend-design` |
| **Hallmark** | [Nutlope/hallmark](https://github.com/nutlope/hallmark) | Corre 57 "slop-test gates" + auto-crítica antes de entregar. Insiste en **variedad estructural**, no solo visual — dos briefs distintos no deberían compartir el mismo ritmo hero→3-features→CTA→footer. Trae 3 verbos explícitos: `hallmark audit <target>` (audita código existente), `hallmark redesign <target>` (mantiene copy/IA, cambia estructura), `hallmark study <screenshot\|URL>` (extrae el ADN de un diseño que te gusta: macroestructura, pairing tipográfico, ancla de color). | `npx skills add nutlope/hallmark` |
| **avoid-ai-design** | [funboy322/avoid-ai-design](https://github.com/funboy322/avoid-ai-design) | Auditor puro (no genera desde cero): revisa código frontend ya generado por IA y lo reescribe quitando gradientes morado-azul, Inter sin criterio, shadcn sin tocar, glassmorphism gratuito. Complemento de "avoid-ai-writing" del mismo autor para copy. | `npx skills add funboy322/avoid-ai-design` |

**Uso real en este proyecto:** se instalaron `frontend-design` y `hallmark` (ambas en `~/.agents/skills/`, symlinkeadas a Claude Code) y se usó `frontend-design` para guiar las mejoras del sitio (ver [[README|README]] → "Aplicado ya en").

## 3. Skills de estilo Apple / HIG

Útiles cuando se quiere que una interfaz (web o nativa) se sienta "Apple-like": tipografía SF, espaciados y radios consistentes con Human Interface Guidelines, soporte light/dark real, componentes con la sobriedad característica de Apple.

| Skill | Autor | Alcance |
| --- | --- | --- |
| [apple-hig-designer-skill-2026](https://github.com/tristan-mcinnis/apple-hig-designer-skill-2026) | tristan-mcinnis | HIG-compliant para iOS/macOS/watchOS/tvOS/visionOS, incluye "Liquid Glass" (el material del último rediseño de Apple). Genera tipografía San Francisco con thresholds Display/Text correctos y colores de sistema con soporte light/dark completo. |
| [apple-design-skill](https://github.com/dickwu/apple-design-skill) | dickwu | Revisor de UI/UX cross-platform basado en HIG — funciona sobre Flutter, Tauri, Electron, React Native (`.dart`, `.tsx`, `.jsx`, `.vue`, `.svelte`, `.swift`). Trae 53 documentos de referencia de guidelines organizados por tema, más un `hig-lookup.md` para enrutar a la sección correcta. Compatible con Claude Code, Cursor, Codex. |
| [claude-code-apple-skills](https://github.com/rshankras/claude-code-apple-skills) | rshankras | Colección más amplia para desarrollo Apple end-to-end (no solo diseño): validación de producto, generación de código, ASO (App Store Optimization). |
| Apple HIG Reference (14 skills) | vía [claudemarketplaces.com](https://claudemarketplaces.com/skills/nexu-io/open-design/apple-hig) | El HIG completo partido en 14 skills por plataforma/tema (foundations, components, input patterns, platform tech) — instalar solo las que apliquen para no saturar el contexto. |

**Nota para un sitio web (no app nativa):** no aplica instalar las skills de SwiftUI/iOS — lo relevante del "estilo Apple" para web es la disciplina tipográfica (jerarquía clara, poco peso decorativo), el uso de espacio en blanco, y transiciones sutiles con propósito — eso ya lo cubre `frontend-design` + los principios HIG generales sin necesitar la skill nativa.

## 4. Skills de animación / interactividad avanzada

| Recurso | Qué cubre |
| --- | --- |
| [freshtechbro/claudedesignskills](https://github.com/freshtechbro/claudedesignskills) | Colección especializada en gráficos 3D, animación e interactividad: Three.js, GSAP, React Three Fiber, Framer Motion, Babylon.js. 27 plugins (22 individuales + 5 bundles). Útil si un proyecto pide algo más "producto interactivo" que un CV estático. |
| [daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) | Marketplace propio con skills "production-ready" para distintos flujos de desarrollo, no solo diseño. |

**Criterio de cuándo usarlas:** motion "sirve a un momento", no decora todo — para un CV de una página como el nuestro, sobra GSAP/Three.js; alcanza con una animación de entrada orquestada (CSS + `IntersectionObserver`, sin dependencias). Se reservan estas skills para proyectos que sí necesiten 3D/canvas/scroll-jacking real.

## 5. Marketplaces y listas curadas (para descubrir más a futuro)

- [anthropics/skills](https://github.com/anthropics/skills) — oficial, calidad más consistente.
- [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) — lista curada ~14k★.
- [ComposioHQ awesome-claude-skills](https://composio.dev/content/top-claude-skills) — la más grande, ~67k★.
- [skills.sh](https://skills.sh) / [skillselion.com](https://skillselion.com) — catálogos con contador de instalaciones (permite ver cuáles son realmente usadas, no solo listadas).
- [claudemarketplaces.com](https://claudemarketplaces.com) — agrupa marketplaces de terceros, incluye bundles temáticos (ej. "Web Wizard": `frontend-design` + `api-design-principles` + `lint-and-validate` + `create-pr`).

## 6. Cómo se instalan y dónde quedan

```bash
# Buscar/instalar por owner/repo (o URL completa + --skill si el repo trae varias)
npx skills add anthropics/skills --skill frontend-design
npx skills add nutlope/hallmark

# Quedan symlinkeadas en:
~/.agents/skills/<nombre>/        # fuente real
~/.claude/skills/<nombre>         # symlink que Claude Code lee
```

Una vez instalada, Claude Code la detecta sola por su descripción (no hace falta invocarla por nombre), o se puede forzar con la herramienta `Skill` / mencionándola explícitamente ("usa hallmark para...").

⚠️ Igual que cualquier código de terceros: revisar `SKILL.md` antes de usarla en algo sensible — corren con permisos completos del agente.
