# AGENTS.md

## Qué es esto

Clon de Asteroids en HTML5 Canvas puro. **Sin paso de build, sin bundler, sin package.json, sin dependencias.** Tres archivos: `index.html`, `game.js`, `favicon.svg`.

## Cómo ejecutarlo

```bash
npx serve .
# luego abre http://localhost:3000
```

O haz doble clic en `index.html` — los navegadores modernos ejecutan la sintaxis `class` de ES6 sin problema, y no hay nada que obtener por HTTP. (`file://` rechaza `favicon.svg` en algunos navegadores, es inofensivo.)

## No agregar tooling

A propósito no hay linter, formateador, typechecker, test runner ni transpilador. No introduzcas uno. Si necesitas verificar un cambio, abre la página y juégala.

## Arquitectura (todo está en `game.js`)

`game.js` tiene ~420 líneas, de arriba a abajo, un módulo por asunto. Las secciones están marcadas con banners `// ── … ──` — lee esos comentarios para navegar.

Estado top-level clave (`game.js:239`):
- `ship, bullets, asteroids, particles` — arrays de entidades
- `score, lives, level`
- `state` — `'playing' | 'dead' | 'gameover'`
- `W = 800`, `H = 600` — tamaño de canvas fijo; si cambias esto, redimensiona el elemento `canvas` en `index.html`

El tamaño de los asteroides usa tres arreglos paralelos indexados por tamaño (1=pequeño, 2=mediano, 3=grande) — `game.js:61-63`:
- `RADII`, `SPEEDS`, `POINTS`

El mundo es toroidal: `wrap(v, max)` en `game.js:27` mantiene todo dentro de la pantalla.

## Gotcha del input

`Space`, `ArrowUp/Down/Left/Right` llaman `e.preventDefault()` (`game.js:15`) para que la página no haga scroll. Si agregas nuevas teclas de movimiento, añádelas también a esa lista, o la página hará scroll bajo el canvas.

`pressed(code)` (`game.js:20`) es por flanco (una vez por keydown); `keys[code]` es por nivel. Usa la correcta — `pressed('Space')` para disparar, `keys['ArrowUp']` para propulsar.

## Cómo agregar cosas

- **Nuevo tipo de entidad**: copia la forma de `Bullet`/`Asteroid`/`Particle` (banderas `update`/`draw`/`dead`), empuja instancias al array correcto en `update()` en `game.js:293`, y aplica `filter(!dead)` después.
- **Nuevo tipo de asteroide / power-up**: mira lo que promete el README sobre "power-ups especiales" y "estrella fugaz" — son promesas documentadas que aún no están implementadas en `game.js`. Si te piden agregarlas, es aquí donde van.
- **Niveles**: `nextLevel()` en `game.js:268`; el conteo de spawn es `3 + level`.

## Copy de UX

El texto del HUD y los overlays (`GAME OVER`, `NIVEL`, `PUNTAJE`) están en español — mantén en español cualquier string nuevo que vea el jugador.