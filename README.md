# VISAPP v0.3

En vivo: **https://xmc223-arch.github.io/visapp-v3/**

**Motor de audio nuevo.** El anterior mandaba al shader el promedio crudo de la FFT, que en música normal vive entre 0.1 y 0.3 — por eso casi no se movía nada. Ahora:

- **AGC por banda**: cada banda lleva su propio pico y piso adaptativos, así que la app se auto-calibra. Música bajita o fuerte reaccionan igual.
- **Ataque rápido, caída lenta** en cada banda: el golpe se siente instantáneo y decae con cuerpo.
- **Detección de transientes** por derivada del bajo normalizado (`uHit`), que dispara patadas radiales, flashes y kicks de zoom.
- **Beat tracking**: mediana de los intervalos entre onsets → BPM, fase de beat, `uBeat` (pulso exponencial), y toggle/random en cada beat. El punto junto al logo parpadea con el beat y el BPM aparece junto a los FPS.
- FFT a 2048 con rango dinámico ajustado (-88 a -22 dB) y poco suavizado interno.

**RELIEF rehecho.** Los knobs antes movían poco porque los rangos eran estrechos y el feedback se comía los cambios. Ahora:

- PUSH controla la profundidad del domain warping de 0.25 a 8 (antes 0.35 a 3.2) más la velocidad de flujo.
- SWIRL es un vórtice real: la rotación crece hacia el centro (`7/(r+0.3)`), no un giro global.
- DETAIL barre de macro a micro (escala 0.45–7.5) y las bandas de contorno van de 3 a 49.
- FEED arrastra el campo con desplazamiento, no solo mezcla → look líquido.
- Paleta coseno iridiscente en vez de dos tonos planos, más fresnel y specular con luz en movimiento.

**CHROME** es nuevo. **Botones** más grandes, con fondo sólido y borde claro para que se lean sobre cualquier visual.

## Controles

**5 knobs** (arrastra vertical; doble tap = reset): PUSH, SWIRL, DETAIL, COLOR, FEED.
**3 faders**: CAM, BASS, HIGH — la barra blanca de adentro es el nivel en vivo.
**Botones**: AUTO (LFOs), RND, LIFT (mantener presionado), FLIP, y % de resolución.
**CAM**: OFF → TEX (el video entra al shader) → SENS (solo movimiento y brillo).

Truco: `#0` … `#3` al final de la URL fuerza la escena al cargar.

## Notas

- Requiere iOS 15+ / WebGL2. Cámara y micrófono solo por HTTPS.
- Buffers RGBA16F cuando el dispositivo lo soporta, con fallback a RGBA8.
- CHROME es la escena más pesada (raymarching): si baja de fps, usa el botón de resolución al 40%.
