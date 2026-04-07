# Manual para cargar imágenes y videos

Este manual explica cómo agregar imágenes a la galería de imágenes y videos a la galería de videos desde el panel de administración del sitio.

## Dónde se editan

- Galería de imágenes: `Multimedia / Resources` -> `Images Gallery`
- Galería de videos: `Multimedia / Resources` -> `Videos Gallery`

## Antes de empezar

- Ten listas las imágenes optimizadas.
- Si vas a publicar un video, ten a mano el enlace de YouTube.
- Revisa siempre los dos idiomas. El sitio usa alemán y español, así que el título, la descripción y los textos visibles deben actualizarse en ambas versiones.

## Cómo agregar una imagen a la galería de imágenes

1. Entra a `/admin`.
2. Abre `Multimedia / Resources`.
3. Entra en `Images Gallery`.
4. Edita primero una versión de idioma y luego la otra.
5. Busca la lista `Gallery Images`.
6. Haz clic en `Add Gallery Images` o en el botón para agregar un nuevo elemento.
7. Completa los campos.
8. Guarda o publica los cambios.

### Campos de cada imagen

- `Show in Gallery`: si está activado, la imagen se muestra. Si lo desactivas, la imagen queda guardada pero oculta.
- `Image`: sube o selecciona el archivo.
- `Title`: título visible en la galería.
- `Description`: texto breve opcional.
- `Alt Text`: texto alternativo para accesibilidad y SEO.

### Recomendaciones para imágenes

- Usa imágenes de al menos `1600 px` en el lado más largo.
- Optimiza el peso antes de subirlas.
- Si puedes, usa nombres de archivo claros y consistentes, por ejemplo `carlos-johnson-recital-berlin.jpg`.
- Mantén el mismo orden en alemán y en español para que ambas galerías coincidan.

## Cómo agregar un video a la galería de videos

1. Entra a `/admin`.
2. Abre `Multimedia / Resources`.
3. Entra en `Videos Gallery`.
4. Edita primero una versión de idioma y luego la otra.
5. Busca la lista `Gallery Videos`.
6. Agrega un nuevo elemento.
7. Completa los campos.
8. Guarda o publica los cambios.

### Campos de cada video

- `Show in Gallery`: controla si el video aparece o queda oculto.
- `Title`: título del video.
- `Publication Date`: fecha de publicación o de referencia.
- `YouTube Video ID`: identificador del video en YouTube.
- `Cover Image (optional)`: miniatura personalizada opcional.
- `Description or Concert Notes`: descripción, programa, intérpretes o notas del concierto.

### Cómo obtener el `YouTube Video ID`

No copies el enlace completo. Usa solo la parte final del enlace.

Ejemplos:

- Si la URL es `https://www.youtube.com/watch?v=MGreSb1NjtA`, debes pegar `MGreSb1NjtA`.
- Si la URL es `https://youtu.be/MGreSb1NjtA`, debes pegar `MGreSb1NjtA`.

### Sobre la portada del video

- La portada es opcional.
- Si subes una imagen en `Cover Image`, esa imagen se usa como miniatura en la galería.
- Si no subes portada, la galería usa automáticamente la miniatura de YouTube.

## Orden, edición y ocultación

- En ambas galerías puedes reordenar los elementos desde la lista del CMS.
- El orden de la lista es el orden en que aparecen en la página.
- Si no quieres borrar un elemento, desactiva `Show in Gallery`.

## Revisión antes de publicar

Antes de cerrar el cambio, revisa esto:

- La imagen o el video aparece en la galería correcta.
- El texto está actualizado en alemán y en español.
- El orden de los elementos es correcto.
- El `Alt Text` de las imágenes está completo.
- El `YouTube Video ID` funciona.
- La portada del video se ve bien, si se agregó una.

## Anexo: guía rápida para imágenes del hero de inicio

Estas recomendaciones aplican al carrusel principal de la home.

- Tamaño ideal: `1600 x 2400 px`
- Tamaño mínimo: `1200 x 1800 px`
- Proporción recomendada: `2:3`
- Mejor resultado: retratos verticales
- Mantén el sujeto centrado o ligeramente a la derecha
- Deja margen de seguridad para el recorte en tablet y móvil
- Peso recomendado: `250 KB` a `500 KB`
- Formato preferido: `WebP`, aunque `JPG` también funciona

Si solo sigues una regla, exporta cada imagen del hero en `1600 x 2400 px` y verifica que siga funcionando bien con recorte centrado.
