# carlosjohnson.github.io

Sitio web bilingüe (alemán / español) del violinista **Carlos Johnson**, construido con Hugo, editable por el propio cliente a través de un CMS git-based y desplegado gratis en GitHub Pages.

Forma parte del proyecto **profi-web**: páginas web para músicos con plantillas reutilizables y un panel de administración para que ellos mismos mantengan el contenido.

- **Producción:** https://pablo-johnson.github.io/carlosjohnson.github.io/
- **Panel de administración:** `/admin` (por ejemplo, https://pablo-johnson.github.io/carlosjohnson.github.io/admin/)
- **Manual para el cliente:** [user-manual.md](user-manual.md)

---

## Stack

| Pieza | Tecnología | Detalle |
|---|---|---|
| Generador | **Hugo extended** | Versión fijada en el workflow: `0.159.1` (requiere Dart Sass) |
| Tema | **PaperMod** | Vendorizado dentro del repo (`themes/PaperMod`), **no** es submódulo |
| CMS | **Sveltia CMS** | `static/admin/`, backend GitHub, config compatible con Decap/Netlify CMS |
| Hosting | **GitHub Pages** | Deploy vía GitHub Actions |
| Formulario | **formsubmit.co** | Envío AJAX, sin backend propio |
| Analítica | **Google Analytics 4** | Con banner de consentimiento propio (Consent Mode) |
| Idiomas | `de` (por defecto) + `es` | `i18n.structure: multiple_files` |

---

## Estructura del repositorio

```
.
├── .github/workflows/hugo.yml     # Build + deploy a GitHub Pages
├── hugo.toml                      # Config del sitio, idiomas y menús
├── archetypes/                    # Plantillas de front matter (concerts.md, default.md)
├── assets/
│   ├── css/extended/              # CSS extra a nivel de proyecto (footer-menu.css)
│   └── images/                    # Media del CMS (pasa por el pipeline de imágenes)
├── content/                       # Contenido en .md, un archivo por idioma
├── data/site_settings.yml         # Ajustes globales editables desde el CMS
├── i18n/                          # (vacío: las traducciones viven en el tema)
├── layouts/                       # Overrides de proyecto (ganan sobre el tema)
│   ├── partials/footer.html
│   ├── partials/responsive-image.html   # Pipeline de imágenes (srcset + WebP)
│   ├── partials/image-url.html          # URL de una imagen procesada
│   └── shortcodes/analytics-consent-manage.html
├── static/
│   └── admin/                     # Sveltia CMS (index.html + config.yml)
├── themes/PaperMod/               # Tema + TODAS las customizaciones
└── user-manual.md                 # Manual de uso para el cliente
```

> **Ojo con la precedencia:** existen dos `footer.html` (uno en `layouts/partials/` y otro en `themes/PaperMod/layouts/partials/`). Hugo usa siempre el de la raíz del proyecto.

---

## Contenido y modelo de datos

### Multilingüe

Alemán es el idioma por defecto y **omite el sufijo** en el nombre de archivo; español lo lleva explícito:

```
content/about.md       → alemán  (/about/)
content/about.es.md    → español (/es/about/)
```

Toda página nueva debe crearse en **ambos** idiomas.

### Páginas y secciones

| Ruta | Tipo | Layout que la renderiza |
|---|---|---|
| `content/_index.md` | Home | `partials/index_profile.html` |
| `content/about.md` | Biografía | `_default/single.html` |
| `content/teaching/_index.md` | Docencia | `teaching/list.html` |
| `content/concerts/` | Conciertos | `concerts/list.html` + `concerts/single.html` |
| `content/images/_index.md` | Galería de imágenes | `images/list.html` |
| `content/videos/_index.md` | Galería de videos | `videos/list.html` |
| `content/videos/detail.md` | Detalle de video | `videos/single.html` |
| `content/audios/_index.md` | Galería de audios | `audios/list.html` |
| `content/contact.md` | Contacto (`type: contact`) | `contact/single.html` |
| `content/privacy.md` | Política de privacidad | `_default/single.html` |

### Patrón clave: galerías como listas en el front matter

Las galerías **no** son carpetas de páginas: son arrays dentro del front matter del `_index` de cada sección (`gallery_items`, `gallery_videos`, `gallery_audios`). Esto permite reordenarlas por drag & drop desde el CMS y ocultar elementos sin borrarlos.

Dos flags recurrentes:

- `enabled` (por ítem): si es `false`, el elemento no se renderiza pero se conserva.
- `show_page` (por sección): si es `false`, la sección desaparece del menú principal y del pie **sin borrar el contenido**. Lo respetan `header.html`, `layouts/partials/footer.html` y los propios layouts de sección.

### Conciertos

Única colección de tipo carpeta. El layout divide automáticamente la agenda comparando `date` con `now`:

- `futureEvents` → sección "Próximos conciertos" (orden ascendente)
- `pastEvents` → sección "Conciertos pasados" (orden descendente)

La home muestra los **3 próximos** eventos. Slug automático: `{{year}}-{{month}}-{{day}}-{{slug}}`.

### Ajustes globales

`data/site_settings.yml` guarda el email de destino del formulario de booking y las URLs de redes sociales. Se consume desde `hugo.Data.site_settings` en `contact/single.html` y desde `site.Data.site_settings` en el footer.

---

## CMS (Sveltia)

`static/admin/index.html` carga el CMS desde CDN:

```html
<script src="https://unpkg.com/@sveltia/cms/dist/sveltia-cms.js"></script>
```

`static/admin/config.yml` (≈380 líneas) define:

- **Backend:** `github`, repo `pablo-johnson/carlosjohnson.github.io`, branch `main`.
- **Media:** se sube a `assets/images` (así pasa por el pipeline de imágenes) y se referencia como `/images/...`.
- **i18n:** `multiple_files`, locales `[de, es]`, default `de`, omitiendo el locale por defecto del nombre de archivo.
- Cada campo declara su comportamiento multilingüe: `i18n: true` (traducible) o `i18n: duplicate` (valor compartido entre idiomas, típico de imágenes, fechas, IDs de YouTube y URLs).

### Colecciones

| Colección | Qué edita |
|---|---|
| **Site Settings** | Email de booking + Facebook / Instagram / YouTube |
| **Homepage** | Hero (eyebrow, título, subtítulo máx. 500 caracteres, carrusel con mín. 3 slides, 3 "profile briefs", segundos de autoplay 3–5) y video destacado |
| **Pages** | Biografía y Docencia (cátedra en Alemania, cátedra en Perú, masterclasses y recursos para alumnos) |
| **Multimedia / Resources** | Galerías de imágenes, videos y audios |
| **Concert Schedule – List Page** | Textos de la página de agenda |
| **Concert Schedule / Events** | Alta y edición de conciertos (`create: true`) |

> **Autenticación:** el `config.yml` no declara `base_url` ni `auth_endpoint`. Antes de replicar el setup en otro sitio conviene documentar/definir cómo se resuelve el login OAuth de GitHub (proxy propio tipo `sveltia-cms-auth`), ya que además exige que el cliente tenga cuenta de GitHub con acceso al repo.

---

## Customizaciones sobre PaperMod

El tema está copiado dentro del repo y **todas las customizaciones viven ahí dentro** (~3.300 líneas propias). Esto implica que actualizar PaperMod desde upstream requiere un merge manual.

### Layouts propios

| Archivo | Función |
|---|---|
| `partials/index_profile.html` | Home completa: hero con carrusel, próximos 3 conciertos, video destacado |
| `partials/header.html` | Reescrito: submenús desplegables, overflow menu, selector de idioma, soporte de `show_page` |
| `partials/footer.html` | Menú de pie con `<details>`, iconos sociales SVG inline, créditos (**el que manda es el de `layouts/` en la raíz**) |
| `partials/google_analytics.html` | Carga GA4 solo en producción y solo con consentimiento |
| `partials/analytics_footer.html` | Banner de consentimiento y tracking de eventos |
| `images/list.html` | Galería con lightbox |
| `videos/list.html` · `videos/single.html` | Listado de videos y página de detalle |
| `audios/list.html` | Biblioteca de audios (YouTube o URL directa) |
| `concerts/list.html` · `concerts/single.html` | Agenda dividida en próximos/pasados y ficha de concierto |
| `teaching/list.html` | Página de docencia con cátedras, masterclasses y recursos |
| `contact/single.html` | Formulario de contacto/booking |

### Assets propios

- **CSS por sección**, inyectado inline con `resources.Get | minify` dentro de cada layout: `homepage.css` (580 líneas), `images.css`, `videos.css`, `audios.css`, `concerts.css`, `teaching.css`, `contact.css`.
- **CSS extendido** (se concatena automáticamente al bundle de PaperMod): `extended/navigation.css`, `extended/analytics-consent.css`, `extended/blank.css`, y a nivel de proyecto `assets/css/extended/footer-menu.css` (sticky footer).
- **JS:** `homepage-carousel.js` (carrusel del hero) e `images-gallery.js` (lightbox).

### Traducciones

Las claves propias se añadieron a `themes/PaperMod/i18n/de.yaml`, `es.yaml` y `en.yaml`. El directorio `i18n/` de la raíz está vacío. Familias de claves: `contact_form_*`, `teaching_*`, `analytics_consent_*`, `gallery_*`, `videos_*`, `audios_*`, `menu_*`, `footer_*`, `upcoming_events`, `past_events`, `more_info`, `view_all_events`, `featured_video`, `developed_by`, `privacy_page_link`.

**Ningún layout contiene texto en un idioma concreto**: todo pasa por `i18n`, incluidos los `aria-label`, los estados vacíos de las galerías y el pie. Añadir un idioma nuevo a un sitio es crear su `.yaml` y declararlo en `hugo.toml`; no hay que tocar plantillas. El inglés está traducido aunque este sitio no lo use, como base para los próximos.

---

## Pipeline de imágenes

El media vive en `assets/images` (no en `static/`), así que Hugo puede procesarlo en tiempo de build: redimensiona, convierte a **WebP** y genera `srcset` + `width`/`height` (esto último evita saltos de layout / CLS).

Dos partials en `layouts/partials/` encapsulan todo:

| Partial | Devuelve |
|---|---|
| `responsive-image.html` | Un `<img>` completo con `srcset`, `sizes`, dimensiones y `loading` |
| `image-url.html` | Solo la URL de una versión procesada (para lightbox, `data-*`, backgrounds) |

```go-html-template
{{ partial "responsive-image.html" (dict
    "src"   $item.image
    "alt"   $alt
    "class" "gallery-img"
    "sizes" "(max-width: 700px) 100vw, 33vw"
    "widths" (slice 400 600 900 1200)) }}

{{ $url := partial "image-url.html" (dict "src" $item.image "width" 1800) }}
```

Parámetros de `responsive-image.html`: `src` (obligatorio), `alt`, `class`, `id`, `sizes`, `widths`, `loading`, `fetchpriority`, `decoding`, `quality`.

Detalles de comportamiento:

- Solo genera anchos **menores** que el original (nunca escala hacia arriba) y añade siempre el tamaño original como mayor entrada del `srcset`.
- **Degrada con elegancia:** si la imagen es remota (p. ej. la miniatura de YouTube de un video sin portada), no existe en `assets/` o no es procesable (SVG, GIF), emite un `<img>` simple con la URL original.
- La primera slide del hero se marca `loading="eager"` + `fetchpriority="high"`; el resto va en `lazy`.
- Las imágenes escritas en markdown también pasan por el pipeline gracias al render hook `_default/_markup/render-image.html`.

Efecto típico: `cj-violin.jpg` pesa 589 KB; un móvil ahora descarga la variante de 600 px en WebP, de ~22 KB.

Consecuencia práctica: **no hace falta optimizar a mano** antes de subir una imagen desde el CMS. Lo único que sigue importando es subirla con resolución suficiente y en la proporción correcta (ver [user-manual.md](user-manual.md)).

---

## Reutilizar la plantilla en otro sitio

Los layouts no contienen datos del cliente. Todo lo específico de un sitio vive en configuración:

| Dónde | Qué |
|---|---|
| `hugo.toml` | `baseURL`, `title` por idioma, idiomas y menús, ID de GA4, `copyright_name`, `[params.developer]` |
| `data/site_settings.yml` | Email de booking y redes sociales (editable desde el CMS) |
| `static/admin/config.yml` | `backend.repo` y `branch` del repositorio destino |
| `static/admin/index.html` | Título de la pestaña del panel (HTML estático, no pasa por Hugo) |
| `content/` + `assets/images/` | Textos e imágenes del músico |

Parámetros del pie:

```toml
[params]
  # Titular del copyright. Si se omite, se usa el title del sitio.
  copyright_name = 'Carlos Johnson'

  # Crédito del desarrollador. Borra el bloque entero para ocultarlo.
  [params.developer]
    name = 'Pablo Johnson'
    url = 'https://github.com/pablo-johnson'
```

Si se borra `[params.developer]`, el crédito desaparece junto con su separador, sin dejar restos en el pie.

---

## Formulario de contacto

`contact/single.html` construye el formulario contra **formsubmit.co** usando el email de `data/site_settings.yml`:

- Acción normal: `https://formsubmit.co/<email>` · Acción AJAX: `https://formsubmit.co/ajax/<email>`
- Envío por `fetch` con estados traducidos (enviando / éxito / error) y `aria-live`.
- Anti-spam: honeypot `_honey` oculto.
- Al enviar con éxito dispara el evento GA4 `contact_form_submit_success`.
- Si `booking_email` está vacío, muestra un aviso en lugar del formulario.

---

## Analítica y consentimiento

Configurado en `hugo.toml`:

```toml
[services]
  [services.googleAnalytics]
    id = 'G-XXXXXXXXXX'
```

Comportamiento:

- Sin `id`, GA4 no se carga y el banner no aparece.
- Solo se activa en producción (`HUGO_ENVIRONMENT=production`).
- `ad_storage` siempre desactivado; `analytics_storage` solo tras aceptar.
- No mide `/admin`.
- Eventos: `page_view`, `outbound_click`, `contact_form_submit_success`, más los `data-analytics-*` declarados en los layouts (`select_content`, `view_item_list`).
- El usuario puede reabrir sus preferencias con el shortcode pareado `{{< analytics-consent-manage >}}Texto del botón{{< /analytics-consent-manage >}}` (usado en `content/privacy.md` y `content/privacy.es.md`).

---

## Desarrollo local

Requisitos: **Hugo extended** `0.159.x` y **Dart Sass**.

```bash
hugo server -D                 # servidor de desarrollo con drafts
hugo server --navigateToChanged
hugo --minify                  # build de producción en ./public
```

`public/` está en `.gitignore`: el sitio se compila en CI, nunca se commitea.

Para editar contenido en local con el CMS hace falta el modo local de Sveltia (servir `/admin` sobre el sitio local); en el flujo actual la edición se hace en producción contra el repo de GitHub.

---

## Despliegue

`.github/workflows/hugo.yml`:

1. Se dispara en cada `push` a `main` (o manualmente con `workflow_dispatch`).
2. Instala Hugo extended `0.159.1` + Dart Sass.
3. `checkout` con `submodules: recursive` y `fetch-depth: 0`.
4. Build con `hugo --minify --baseURL "${{ steps.pages.outputs.base_url }}/"` y `HUGO_ENVIRONMENT=production`.
5. Publica `./public` en GitHub Pages.

Cada cambio guardado desde `/admin` es un commit en `main`, así que **publicar desde el CMS dispara automáticamente el deploy** (≈1–2 minutos).

El `baseURL` de `hugo.toml` apunta a la URL de project page, pero el workflow lo sobrescribe con el valor real de Pages. Para un dominio propio habría que añadir `static/CNAME` y configurar el DNS.

---

## Notas de mantenimiento

- **Actualizar PaperMod** implica merge manual: el tema está vendorizado y modificado in situ.
- **El detalle de video es client-side**: `videos/single.html` renderiza todos los videos ocultos y JavaScript muestra el seleccionado. No genera una URL propia por video.
- **Los menús se definen en `hugo.toml`**, no son editables desde el CMS. El cliente solo puede ocultar secciones vía `show_page`. Es config por sitio, no un hardcodeo de layout, pero sigue siendo un archivo que hay que tocar a mano en cada alta.
- **El footer del tema** (`themes/PaperMod/layouts/partials/footer.html`) es código muerto: el override de `layouts/` siempre gana. Conserva el crédito modificado de PaperMod.
- Al mover el media, `static/images/` quedó como carpeta vacía en el disco local: se puede borrar, git no la trackea.
