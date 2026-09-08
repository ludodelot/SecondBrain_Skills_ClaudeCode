---
type: skill-note
skill: "Claude Code (skills, marketplaces, workflows)"
context: "Conocimiento acumulado sobre Claude Code — sobre todo skills de terceros/oficiales para mejorar diseño web (evitar 'AI slop', estilo Apple/HIG, animación) y cómo instalarlas/usarlas."
tags: [claude-code, ai-agents, web-design, developer-tools, skills]
github_mirror: "https://github.com/ludodelot/SecondBrain_Skills_ClaudeCode"
---

# Claude Code — Skill (se va ampliando)

Esta carpeta se mantiene siempre reflejada (push) en el repo público
[**SecondBrain_Skills_ClaudeCode**](https://github.com/ludodelot/SecondBrain_Skills_ClaudeCode) — mismo patrón que [[04_Knowledge/Skills/GitHub/README|Skills/GitHub]].

**Idea:** cada vez que se descubra/instale una skill útil de Claude Code (oficial o de la comunidad), o un patrón de uso, se documenta aquí — no se pierde en un chat.

## Archivos

| Archivo | Para qué sirve |
| --- | --- |
| `Web_Design_Skills_Catalogo.md` | Investigación 2026-09-08: catálogo curado de skills de Claude Code para **mejorar el diseño de sitios web** — anti "AI slop" (Hallmark, avoid-ai-design), estilo Apple/HIG, animación (Three.js/GSAP/Framer Motion), marketplaces curados. Incluye cómo instalarlas y cuándo usar cada una. |
| `Skills_Instaladas.md` | Registro de qué skills están instaladas en esta máquina (`~/.agents/skills/`), quién las publica, y en qué proyecto se aplicaron. |
| `Iconos_Aurora_Marquee_Research.md` | Investigación 2026-09-08 (3ª ronda): por qué Lucide gana a Phosphor para estilo Apple, cómo construir un sprite de iconos SVG inline sin librería externa, fondo "aurora" animado con CSS puro, y marquee/carrusel infinito sin dependencias — con el código exacto aplicado al CV. |
| `Deep_Research_Mejores_Webs_2026.md` | Investigación 2026-09-08 (2ª ronda, más profunda): **proceso** real de "Claude Code para diseñadores" (plan→dev server→screenshot loop→stacking de skills), catálogo de librerías de componentes mejor valoradas (shadcn/ui, Magic UI, Aceternity UI), librerías de animación (Motion, GSAP, AOS), y tendencias de diseño 2026 (bento grid, scrollytelling, sitios premiados en Awwwards/Web Design Awards). Incluye conclusión operativa para el próximo rediseño del CV. |

## Conceptos base

- Una **skill** de Claude Code es una carpeta con un `SKILL.md` (instrucciones en markdown + opcionalmente `references/` y scripts) que Claude carga cuando detecta que aplica a la tarea — mismo formato universal que usan Cursor/Codex/Antigravity.
- Se instalan con `npx skills add <owner>/<repo>` (o una URL de GitHub + `--skill <nombre>` si el repo trae varias) — el CLI `skills` las symlinkea a `~/.claude/skills/` (y a los demás agentes compatibles si existen).
- El repo oficial de Anthropic con skills mantenidas por ellos es [anthropics/skills](https://github.com/anthropics/skills).
- Listas curadas grandes: [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) (~14k★) y [ComposioHQ/awesome-claude-skills](https://composio.dev/content/top-claude-skills) (~67k★).

## Aplicado ya en
- [ludodelot.github.io](https://github.com/ludodelot/ludodelot.github.io) — se usó la skill `frontend-design` (oficial) para guiar mejoras de diseño (dark mode toggle real, animación de entrada única, favicon, PDF export) sin caer en los defaults genéricos de IA.
- 2026-09-08 (2ª iteración) — rediseño completo estilo **Apple "Liquid Glass"** (`backdrop-filter` blur+saturate en nav/hero/cards), nav flotante tipo pill, timeline vertical para experiencia, orbes decorativos en el hero. Investigación de la técnica en [[Web_Design_Skills_Catalogo|catálogo]] §7 (nuevo).
