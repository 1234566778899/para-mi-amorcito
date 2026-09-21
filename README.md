# flores-amarillas 🌻

Una tarjeta web —mobile first— con flores amarillas para el **21 de septiembre**,
inspirada en la plantilla de papelería: fondo amarillo con girasoles, "Te amo" en
script, la carta justificada, la tira de fotos tipo polaroid y el sticker
"eres increíble".

## Contenido

- `src/routes/+page.svelte` — la tarjeta completa (layout, carta, tira de fotos).
- `src/lib/components/Sunflower.svelte` — girasol en SVG (pétalos generados, centro con semillas).
- `src/lib/components/Polaroid.svelte` — marco polaroid para foto o video; al tocarlo se abre a pantalla completa.
- `src/lib/components/Lightbox.svelte` — la vista a pantalla completa (flechas, swipe, Escape, clic fuera para cerrar).
- `src/lib/components/Petals.svelte` — pétalos que caen sobre la página.
- `static/media/` — las fotos y el video: `foto-1.jpeg`, `video-1.mp4`, `foto-2.jpeg`.

Para cambiar las fotos basta con reemplazar los archivos de `static/media/`
(en el abanico se recortan a 4:5; a pantalla completa se ven enteras) o editar
la lista `recuerdos` en `+page.svelte`.

Todo cabe en una sola pantalla: la tarjeta mide `100dvh` y no hay scroll (salvo
en pantallas de menos de 520 px de alto, donde vuelve a habilitarse).

## Desarrollo

```sh
npm install
npm run dev -- --open
```

## Compilar

```sh
npm run build
npm run preview
```

El proyecto usa `@sveltejs/adapter-auto`: al desplegar en Vercel, Netlify o
Cloudflare se configura solo. Para un hosting estático (GitHub Pages, por
ejemplo) hay que instalar `@sveltejs/adapter-static`.

El QR de la raíz (`qr-flores-amarillas.png` / `.svg`) apunta al sitio ya publicado.
