# Bloc de notas

Biblioteca personal de escritos de Diego. Un librero en un escritorio: cada
libro es un relato, y al tomarlo se abre sobre la mesa y se lee pasando hojas.

Sitio estático. Sin servidor, sin base de datos, sin dependencias: HTML, CSS y
JavaScript en un solo archivo.

---

## Estructura

```
index.html              El sitio entero: librero, libro y relatos incrustados
relatos/                Cada relato, también como página suelta
  no-te-vas-a-quedar.html
  luanna-y-mateo.html
img/
  bg-escritorio_luz.jpg     fondo de día
  bg-escritorio.jpg         fondo de atardecer (paso intermedio)
  bg-escritorio_noche.jpg   fondo de noche
  book-open.png             el libro abierto
  hoja-izq.png              hoja que gira, lado izquierdo
  hoja-der.png              hoja que gira, lado derecho
  lomos/                    lomo de cada libro del estante
  lm-01.jpg … lm-07.jpg     fotografías del relato «Luanna y Mateo»

404.html                página muda para rutas que no existen
robots.txt · _headers   bloqueo de buscadores y cabeceras del despliegue
```

## Cómo verlo

Abriendo `index.html` con doble clic funciona: el texto de los relatos va
incrustado dentro del archivo.

Para ver también las páginas sueltas de `relatos/`, hace falta un servidor,
porque cargan por `fetch` y el navegador lo bloquea sobre `file://`:

```bash
python -m http.server 8765
```

Y entrar a `http://localhost:8765/`.

## Añadir un relato

1. Copiar `relatos/luanna-y-mateo.html` y reemplazar el contenido de
   `<div class="narrative">`. Cambiar también el `<title>` y la portada.
2. Recortar el lomo del libro a unos 560 px de alto, con fondo transparente,
   y guardarlo en `img/lomos/`.
3. En `index.html`, añadir una entrada al array `stories`:

```js
{
  id:       'mi-relato',
  title:    'Título',
  subtitle: 'Subtítulo',
  author:   'Diego Dávila',
  excerpt:  'Primeras líneas, por si el texto no carga.',
  date:     'Sep 2026',
  readTime: '10 min',
  href:     'relatos/mi-relato.html',
  spine:    'img/lomos/mi-relato.png',
  ratio:    0.18,   // ancho ÷ alto de la imagen del lomo
  height:   85,     // alto en la balda, en %
}
```

4. Copiar el contenido de `<div class="narrative">` dentro de un
   `<script type="text/html" class="story-source" data-story="mi-relato">`
   en `index.html`, para que el libro funcione sin servidor.

Un libro sin texto todavía se marca con `pending: true` y se abre diciendo que
aún se está escribiendo.

## Detalles del funcionamiento

- **Paginación**: cada página se compone midiendo una hoja real. Cuando un
  párrafo no cabe entero se corta por la última palabra que quepa, y nunca se
  deja una línea suelta al pasar de página.
- **Fotografías**: flotan dentro del texto y alternan de lado. Si una no cabe,
  abre la página siguiente y el texto sigue llenando la actual.
- **El giro**: la hoja es la misma imagen que las hojas dibujadas en el libro,
  así que al posarse cae sobre una copia idéntica de sí misma.
- **Luz**: día de 06:00 a 18:00 en hora de Perú (GMT-5). El botón de sol y luna
  la cambia a mano; si la elección coincide con la hora real, vuelve a
  automático.
- **En móvil**: el libro necesita apaisado. En vertical se pide girar el
  teléfono.

## Publicar

GitHub Pages sirve la raíz del repositorio tal cual: **Settings → Pages**,
rama `main`, carpeta `/ (root)`.

El sitio no se indexa (`robots.txt` y etiquetas `noindex`), pero es accesible
para cualquiera que tenga la dirección.
