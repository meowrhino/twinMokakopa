# Cambios para aplicar en mokakopa

Registro de cambios realizados en twinMokakopa que deben replicarse (con adaptaciones RTL->LTR) en mokakopa.

---

## 1. About: Toggle Statement / CV (APLICAR)

El about ahora tiene un toggle entre "Statement" y "CV". Por defecto muestra el statement. Transición smooth con fade (opacity 0.3s).

**data.json:**
- Nuevo campo `"cv"` a nivel raíz: array de secciones, cada una con `nombreES`, `nombreEN`, `nombreCAT` e `items` (array de `{ fecha, titulo }`)
- Nuevo campo `"contacto"` a nivel raíz: `{ nombre, email, instagram }`
- About mantiene estructura `textosES/EN/CAT` para el statement

**index.html:**
- Eliminar `#language-selector` del `#site-header` (se mueve al about)
- Añadir dentro de `#about-content`: `#about-text`, `#about-switch`, `.lang-switch`, `#about-contact`, `.about-footer`

**css/style.css:**
- Eliminar estilos de `#language-selector`
- Añadir estilos: `#about-text` (con transition opacity), `#about-switch`, `.lang-switch`, `.cv-section` (text-align: left), `#about-contact`
- Eliminar `#language-selector` del media query mobile

**js/main.js:**
- Eliminar `initLanguageSelector()` y su llamada en `init()`
- Añadir `createLangSwitch(container)` y `syncAllLangSwitches()` como componente reutilizable
- Refactorizar `renderAboutContent()` en: `renderAboutContent()`, `renderAboutView()` (con fade transition), `renderStatement()`, `renderCV()`, `renderContact()`
- Variable global `currentAboutView` para el estado del toggle
- Añadir listener de click en `#about-switch` dentro de `initAboutOverlay()`

## 2. Selector de idioma reutilizable en galerías (APLICAR)

El selector ES/EN/CAT ahora es un componente reutilizable via `createLangSwitch(container)`. Se instancia en:
- El about overlay (bajo el toggle statement/CV)
- Al final de cada bloque de texto de galería

Todas las instancias comparten `currentLang` y se sincronizan globalmente.

**js/main.js:**
- `addTextToGallery()`: añade `.lang-switch` al final del bloque de texto
- `updateAllTexts()`: preserva el lang switch al re-renderizar párrafos

**css/style.css:**
- `.gallery-text .lang-switch`: justify-content adaptado (flex-end en RTL, flex-start en LTR)

## 3. Contacto en el about (APLICAR con datos de Monica)

Debajo del toggle statement/CV aparece la info de contacto. En mokakopa usar los datos de Monica en vez de Ana.

## 4. Textos de proyectos multilingüe (APLICAR estructura, NO contenido)

Los textos de cada proyecto ahora tienen traducciones EN/CAT además de ES. Mokakopa debe preparar la misma estructura `textosES/EN/CAT` para sus proyectos (con sus propios textos/traducciones).

## 5. Rediseño bloque de texto de galería (APLICAR)

El bloque de texto al final de cada galería ahora tiene un contenedor scrollable con gradientes fade. El texto está alineado a la izquierda.

**css/style.css:**
- `.gallery-text`: contenedor fullscreen (100dvw × 100dvh), flex centrado, `scroll-snap-align: center`, fondo blanco
- `.gallery-text-scroll` (nuevo): wrapper scrollable con `height: 80dvh`, `max-width: 560px`, `overflow-y: auto`, `padding: 0 5dvw`, `text-align: left`, scrollbar oculta
- Pseudo-elementos `::before`/`::after` con `position: sticky`, gradientes `#fff → transparent` (3rem altura), controlados por clases `.can-scroll-up`/`.can-scroll-down` via opacity
- `.gallery-text .lang-switch`: fuera del scroll wrapper, debajo, `margin-top: 12px`, centrado
- Mobile: `padding: 0 5dvw` en el scroll wrapper

**js/main.js:**
- `addTextToGallery()`: nueva estructura DOM → `.gallery-text` > `.gallery-text-scroll` (h2 + párrafos) + `.lang-switch`
- Nueva función `initScrollGradients(scrollEl)`: listener de scroll que toglea `.can-scroll-up`/`.can-scroll-down` según posición de scroll
- `updateAllTexts()`: actualiza contenido dentro de `.gallery-text-scroll`, resetea scroll y recalcula gradientes

---

## Cambios que NO aplican a mokakopa

- Nombre "Ana López" (mokakopa mantiene "Monica Kopatschek")
- Dominio www.analopezserrano.com (mokakopa mantiene su dominio)
- Contacto de Ana (mokakopa usa contacto de Monica)
- Statement de Ana (mokakopa mantiene el de Monica)
- CV de Ana (mokakopa tendría el CV de Monica)
- Metadata SEO con datos de Ana
- Proyectos eliminados (mokakopa mantiene sus 7 proyectos)
- Textos nuevos de teatroPlantas, porSiglos, bacanales (mokakopa mantiene los suyos)
