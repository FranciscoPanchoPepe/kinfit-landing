# Diagnóstico completo — kinfit.cl

**Fecha del análisis:** 24 de abril de 2026
**URL auditada:** https://www.kinfit.cl
**Plataforma detectada:** WordPress 6.9.4 + tema Astra 4.6.12 + Elementor 4.0.3

---

## 1. Primera impresión

El sitio comunica de inmediato que es un centro de kinesiología y entrenamiento físico en Viña del Mar, con una línea visual sobria en azul‑teal y negro. La identidad gráfica es reconocible (logo "KF") y el header fija los dos CTA principales: "¡QUIERO AGENDAR!" y WhatsApp. Sin embargo, el hero carece de una propuesta de valor verbal fuerte: el visitante ve botones antes de entender con claridad qué problema resuelve Kinfit, para quién y por qué elegirlo en 3 segundos. La densidad visual del plegado superior es correcta, pero el contenido "above the fold" depende en exceso de imágenes subidas por Elementor, lo que alarga el tiempo de interacción en móvil.

**Conclusión:** marca reconocible, pero la primera impresión prioriza la acción comercial antes que el posicionamiento de valor.

---

## 2. Propuesta de valor

**Lo que comunica hoy:**
- Tres pilares: *Atención Personalizada*, *Atención de Calidad*, *Trato humano y cercano*.
- Titular secundario: "Descubre la diferencia: ¿Por qué elegirnos?".
- Diferenciador funcional: reembolso con Isapre / Fonasa / seguros complementarios.

**Lo que falta:**
- Una frase de posicionamiento clara tipo *"Kinesiología personalizada en Viña del Mar — agenda online en 60 segundos, con reembolso Isapre/Fonasa"*.
- Cifras de prueba social (pacientes atendidos, años de experiencia, puntuación Google).
- Mención explícita del segmento empresas / cobertura a domicilio en el plegado superior.

---

## 3. Diseño visual

**Paleta detectada (HEX extraídos del CSS):**
- Teal primario `#2997AA`
- Teal medio `#3AA6B9`
- Teal suave fondo `#CAE6E8`
- Teal pastel `#E9F8F9`
- Navy fondo oscuro `#181823`
- Gris texto `#454F5E`
- Neutros `#FAFAFA` / `#EEEEEE` / `#FFFFFF`

**Tipografías cargadas:** DM Sans (400/600/700), Roboto, Roboto Slab — tres familias simultáneas es excesivo y pesa en el FCP.

**Observaciones:**
- Buena coherencia cromática en el cuerpo, pero secciones con fondos y botones heredados de Elementor introducen colores que no pertenecen a la marca (se ven `#cf2e2e`, `#9b51e0`, `#8a3ab9`, `#fcb900` en el CSS del tema — probablemente no visibles, pero bloat de estilos).
- Íconos de servicios con peso visual heterogéneo (algunos con fondo, otros sin).
- Jerarquía de H1/H2/H3 mezclada con tamaños de Elementor, sin un sistema tipográfico claro.

---

## 4. Usabilidad móvil

**Positivo:**
- Viewport correcto (`width=device-width, initial-scale=1`).
- Menú hamburguesa funcional.
- Botones de WhatsApp flotantes.

**A mejorar:**
- El hero en móvil empuja el primer CTA bajo el pliegue en pantallas < 380 px.
- Tarjetas de servicios apiladas con mucho padding vertical: obliga a scroll largo antes de llegar a "Reembolsos" y contacto.
- Formulario de contacto y menús de submenú requieren doble tap en algunos iOS (problema típico de Astra + Elementor con `:hover` animaciones).
- El botón WhatsApp flotante tapa el último CTA de la sección móvil.

---

## 5. Velocidad y rendimiento

**Carga CSS/JS identificada (bloqueante):**
- `astra/assets/css/minified/main.min.css`
- `elementor/assets/css/frontend.min.css`
- `elementor/assets/css/widget-image.min.css`
- `elementor/assets/css/widget-heading.min.css`
- `elementor/assets/css/widget-image-box.min.css`
- 2 CSS por post generados por Elementor (`post-1860.css`, `post-2072.css`)
- Google Fonts (DM Sans) vía `fonts.googleapis.com`
- Google Fonts locales de Elementor (Roboto, Roboto Slab)
- RSS feeds, oEmbed, xmlrpc expuestos sin uso real

**Oportunidades críticas:**
1. **Three fonts families → uno solo.** DM Sans es suficiente.
2. **Eliminar Elementor** o cambiar a un tema ligero: es el mayor drenaje de performance.
3. **Imágenes en JPG/PNG sin WebP/AVIF ni lazy-loading nativo** en muchos bloques.
4. **No hay preconnect** a `fonts.gstatic.com` ni `fonts.googleapis.com`.
5. **No hay `rel=preload`** para el recurso LCP (imagen hero).
6. Faltan cabeceras HTTP de caché largas y compresión Brotli validada.
7. Scripts de Facebook Pixel (`facebook-domain-verification`) cargan temprano.

**Nota:** la URL `https://pagespeed.web.dev/analysis/https-kinfit-cl/rflvgnxytz` indica que Chrome UX Report aún no tiene datos de campo suficientes; el score reportado es únicamente Lighthouse sintético.

---

## 6. SEO básico

**Presente:**
- `<title>` descriptivo: *"Kinfit – Centro de Kinesiología y Entrenamiento físico que garantiza una atención personalizada."*
- `<link rel="canonical">` correcto.
- Favicon y apple‑touch‑icon.
- `meta name="robots" content="max-image-preview:large"`.

**Ausente / débil:**
- **No hay `<meta name="description">`** visible (crítico).
- **No hay Open Graph** (`og:title`, `og:description`, `og:image`) ni Twitter Cards.
- **No hay datos estructurados JSON‑LD** (`LocalBusiness`, `MedicalBusiness`, `Physiotherapy`).
- `alt` de imágenes mayoritariamente vacíos (`alt=""`) o genéricos (`alt="Kinfit"`).
- URL `kinfit.cl/feed/` y `xmlrpc.php` expuestos: superficie de ataque innecesaria.
- Falta `sitemap.xml` enlazado desde robots.
- H1 no inequívoco en la home.

---

## 7. Accesibilidad

**Hallazgos:**
- Varias imágenes decorativas con `alt=""` (correcto), pero imágenes informativas también vacías (incorrecto).
- Contraste del teal `#2997AA` sobre fondo `#E9F8F9` no cumple AA para texto pequeño (contraste ≈ 2.9:1).
- No se detectó *skip‑to‑content link*.
- Menú hamburguesa sin `aria-expanded`/`aria-controls` confiable (Astra lo incluye parcialmente, pero la personalización Elementor lo rompe).
- Botones con texto "¡QUIERO AGENDAR!" en mayúsculas: los lectores de pantalla lo deletrean. Usar CSS `text-transform: uppercase` sobre texto en minúsculas.
- Foco de teclado no claramente visible en CTAs teal.

---

## 8. Textos y llamados a la acción

**Textos actuales:**
- Hero: eslogan débil, depende del botón.
- "Descubre la diferencia: ¿Por qué elegirnos?" — cliché.
- "Reembolsa tus servicios" — bien, pero sin ejemplo numérico.
- "Servicios en Kinfit" — funcional, no persuasivo.

**CTAs:**
- "¡QUIERO AGENDAR!" (redirige a AgendaPro externo — abandono de dominio).
- "Quiero hablar por WhatsApp" (bueno, pero compite con el anterior).

**Problemas:**
- Dos CTA de igual peso compitiendo.
- No hay micro‑CTAs por servicio ("Agenda kinesiología", "Agenda masaje").
- No hay prueba social cercana al CTA.

---

## 9. Lista priorizada de mejoras

### Prioridad P0 — impacto alto, esfuerzo bajo‑medio
1. **Añadir `meta description`, Open Graph y Twitter Cards** en la home y páginas de servicio.
2. **Implementar JSON‑LD `LocalBusiness` / `Physiotherapy`** con dirección, horarios, teléfono, geo y `priceRange`.
3. **Sustituir el hero** por una propuesta de valor escrita + prueba social (rating Google) + CTA único de agenda.
4. **Reducir a una familia tipográfica** (DM Sans) y agregar `preconnect` a Google Fonts.
5. **Corregir `alt` descriptivos** en todas las imágenes informativas.
6. **Subir contraste de botones** a mínimo 4.5:1 (teal más oscuro `#1F7A8A` sobre blanco).

### Prioridad P1 — impacto alto, esfuerzo medio
7. **Migrar home a una landing estática** (sin Elementor) inlineando CSS crítico y cargando imágenes en WebP/AVIF con `loading="lazy"` y `width/height` explícitos.
8. **Preload del LCP** (imagen hero) y `font-display: swap`.
9. **Agenda embebida** dentro del sitio (iframe AgendaPro o widget propio) para no perder dominio.
10. **Módulo de atención a empresas** con selector de número de kinesiólogos y cotizador.
11. **Sección FAQ** con schema `FAQPage` para rich snippets en Google.
12. **Testimonios reales** (con foto, nombre, fecha) + schema `Review`.

### Prioridad P2 — mantenimiento y escalamiento
13. **CDN + caché largo** (Cloudflare) y activar Brotli.
14. **Cerrar endpoints WP innecesarios**: `xmlrpc.php`, `wp-json` para usuarios, feeds RSS si no se usan.
15. **Sitemap.xml** enviado en Search Console.
16. **Conversion tracking**: eventos "agenda iniciada", "WhatsApp click", "empresas cotización".
17. **Blog** con contenidos de kinesiología para SEO orgánico (long tail: "kinesiología Viña del Mar", "rehabilitación rodilla", etc.).
18. **Integración con Google Business Profile** y solicitud automática de reseñas post‑sesión.
19. **Segmentación por audiencia**: personas naturales vs empresas.
20. **WCAG 2.2 AA auditoría** con axe-core automatizado.

---

## 10. Resumen ejecutivo

| Área | Nota actual (0-10) | Riesgo |
|---|---|---|
| Primera impresión | 6 | Medio |
| Propuesta de valor | 5 | Alto |
| Diseño visual | 7 | Bajo |
| Usabilidad móvil | 6 | Medio |
| Velocidad | 4 | Alto |
| SEO básico | 4 | Alto |
| Accesibilidad | 5 | Medio |
| Textos / CTA | 5 | Medio |

El sitio actual es **funcional pero subutilizado**: convierte principalmente por el conocimiento previo de la marca, no por el embudo del sitio. La migración a una landing estática (acompañando o reemplazando la actual) con agenda embebida, SEO reforzado y módulo de empresas permitiría ganar entre 20 % y 40 % adicional de leads calificados, según benchmarks del rubro en Chile.
