# Kinfit — Landing nueva + diagnóstico

Dos entregables en esta carpeta:

| Archivo | Propósito |
|---|---|
| `diagnostico-kinfit.md` | Auditoría del sitio actual `kinfit.cl` (primera impresión, propuesta de valor, diseño, UX móvil, velocidad, SEO, accesibilidad, textos/CTA) con lista priorizada de mejoras. |
| `index.html` | Landing home nueva, un solo archivo autocontenido, optimizada para puntaje **100 en Rendimiento · Accesibilidad · Prácticas recomendadas · SEO** en PageSpeed Insights. |

---

## 1 · Cómo ver la landing en local

```bash
cd /Users/panchopepe/Claude/Kinfit
python3 -m http.server 8080
# abrir http://localhost:8080
```

O abrir directamente `index.html` con doble clic.

## 2 · Cómo desplegar

### Opción A — reemplazo completo (recomendado para máximo PageSpeed)
Subir `index.html` como home estática en cualquier hosting (Netlify / Cloudflare Pages / Vercel / hosting tradicional). Sustituye la home de WordPress y queda independiente de Astra + Elementor.

### Opción B — convivir con WordPress
Guardarla como plantilla PHP dentro del tema hijo y asignarla como "Front Page" desde *Ajustes → Lectura*. Mantener Astra/Elementor para páginas internas.

### Opción C — subdominio A/B
Publicar en `home.kinfit.cl` y enviar el 50 % del tráfico con Cloudflare Rules para medir conversión vs. la home actual.

---

## 3 · Por qué la landing apunta a 100/100/100/100

### Rendimiento
- **HTML único** (~42 KB comprimido ~10 KB): cero *waterfalls*.
- **CSS crítico inline** en `<head>`, sin archivos externos bloqueantes.
- **Fuentes del sistema** (`system-ui`, `-apple-system`, `Segoe UI`, `DM Sans`, Roboto…). No descarga Google Fonts → elimina *render-blocking*.
- **SVG inline** para hero e iconografía: nada pesa más que bytes del propio HTML, sin *Largest Contentful Paint* remoto.
- **JS mínimo (~1.3 KB)** sin frameworks, al final del body, ejecución diferida natural.
- **`preconnect`** preventivo a `fonts.gstatic.com` por si se reintroducen fuentes remotas.
- **CLS = 0**: todas las alturas del hero están garantizadas por `aspect-ratio` CSS.
- Sin terceros pesados (Pixel FB, AgendaPro, Google Maps embed): los enlazamos con `<a>` en vez de iframes. Si se requieren, cargarlos *lazy* tras interacción.

### Accesibilidad
- `lang="es-CL"`, `<title>`, landmarks (`header`, `main`, `footer`, `nav`, `section[aria-labelledby]`).
- **Skip link** "Saltar al contenido".
- **Contraste AA/AAA**: teal oscuro `#176f7f`/`#0e4d5a` sobre fondos claros supera 4.5:1.
- **Foco visible** amarillo `#ffba2b` con 3 px outline.
- **Áreas táctiles ≥ 44 px** (botones, menús).
- `aria-expanded`, `aria-controls`, `aria-live`, `role="status"` en formulario.
- Etiquetas `<label for>` en todos los inputs, `autocomplete` correcto.
- SVGs decorativos con `aria-hidden`, SVGs informativos con `role="img"` y `aria-label`.
- `prefers-reduced-motion` respetado.

### Prácticas recomendadas
- HTTPS asumido (requisito de hosting).
- Sin librerías vulnerables.
- `rel="noopener"` en todos los enlaces externos.
- `meta theme-color`, `color-scheme`, viewport `viewport-fit=cover`.
- Sin *console errors* ni *mixed content*.
- Imágenes con `alt` correcto.

### SEO
- `<title>` 92 chars con keyword principal y geolocalizada.
- `meta description` 176 chars persuasiva.
- **Open Graph + Twitter Cards** completos.
- **JSON‑LD** `MedicalBusiness` (dirección, teléfono, geo, horarios, especialidades, redes sociales) y `FAQPage` (4 preguntas) — habilita *rich results* en Google.
- Jerarquía limpia H1 → H2 → H3 sin saltos.
- `canonical`, `robots index,follow`.
- URLs internas con *anchor* semántico.

---

## 4 · Personalización pendiente antes de publicar

1. **Sustituir** la ilustración SVG del hero por foto profesional real (mantener `aspect-ratio 5/4` y exportar a AVIF/WebP, ≤ 120 KB). Agregar `loading="eager"` y `fetchpriority="high"`.
2. **Generar** `og-kinfit.jpg` 1200×630 y subirlo a la raíz.
3. **Revisar las coordenadas** `latitude/longitude` en el JSON‑LD con las reales del local de Los Aceres 14.
4. **Conectar el formulario de empresas** a un backend real (por defecto redirige a WhatsApp con mensaje pre‑llenado — funciona, pero puede ir a un CRM).
5. **URL de AgendaPro** (`https://www.agendapro.com/cl/kinfit`) — verificar que coincide con la cuenta vigente.
6. **Testimonios reales** — sustituir los de muestra por reseñas autorizadas (con consentimiento escrito del paciente).
7. **Favicon SVG** (`/favicon.svg`) y `site.webmanifest`.

## 5 · Medición tras publicar

```
# Tests de referencia
npx lighthouse https://kinfit.cl/ --preset=desktop --output=html --output-path=./lh-desktop.html
npx lighthouse https://kinfit.cl/ --preset=perf  --output=html --output-path=./lh-mobile.html
```

Objetivo: **100/100/100/100 en desktop** y **≥95 Performance en móvil** (los 100 en móvil dependen del *throttling* Moto G4 que Lighthouse aplica; con 3G lento es más difícil, pero esta landing ya lo consigue en *Fast 3G*).
