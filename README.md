# Registro Piezas Laser

PWA (app web para móvil) para registrar piezas fabricadas/expedidas con foto.
Sin backend: los datos se guardan solo en el navegador de cada móvil (IndexedDB);
publicar la app NO publica los datos.

## Por qué hay que servirla desde una dirección https
Abrir el `.html` como archivo en el móvil no funciona de forma fiable: los visores de
WhatsApp, Gmail o la app Archivos no ejecutan el JavaScript o no dejan guardar datos.
Servida por https se abre en cualquier móvil, se instala como app y funciona sin conexión.

## Publicar sin que aparezca tu nombre (Netlify Drop)
1. Entra en https://app.netlify.com/drop (cuenta gratuita, puede ser con cualquier email).
2. Arrastra la carpeta `registro-piezas-laser` (el zip descomprimido) a la página.
3. En *Site configuration → Change site name* pon un nombre neutro, p. ej.
   `registro-piezas-hg` → `https://registro-piezas-hg.netlify.app`.
4. Para actualizar: *Deploys* → arrastra de nuevo la carpeta.

## Instalar en el móvil
- **Android (Chrome):** abre la dirección → menú ⋮ → *Instalar aplicación*.
- **iPhone (Safari):** abre la dirección → Compartir → *Añadir a pantalla de inicio*.

## Archivos
- `index.html` — toda la app (HTML, CSS y JS).
- `manifest.webmanifest` — nombre, colores e iconos para instalarla.
- `sw.js` — service worker (funcionamiento sin conexión). Al cambiar la app, sube `CACHE` (`registro-piezas-v2`, ...).
- `icons/` — iconos de la app.

## Actualizaciones sin perder datos
Los registros se guardan en IndexedDB (`piezasDB` / `registros`) en cada móvil y no se
tocan al publicar una versión nueva. Reglas para cada cambio:
- No cambiar nunca `DB_NAME` ni `STORE`, ni borrar el almacén.
- Campos nuevos: el código debe funcionar si faltan en registros antiguos (ver `fotosDe`).
- Si hiciera falta cambiar la estructura: subir `DB_VERSION` y migrar en `onupgradeneeded` sin borrar.
- Subir la versión de `CACHE` en `sw.js` en cada publicación.
