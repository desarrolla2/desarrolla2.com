# CLAUDE.md

Sitio **estático memorial** del blog "Memorias de un desarrollador" (desarrolla2.com): última publicación en 2019, blog cerrado en 2026. Ver `README.md` para estructura, stack y uso.

## Convenciones que NO son obvias

- **Es un memorial cerrado.** No reactivar el blog dinámico ni añadir funcionalidad nueva. El trabajo es edición de `index.html` y mantenimiento del archivo.
- **Cero build, cero dependencias externas en runtime.** Nada de npm, bundlers ni CDN. Bootstrap 5.3 y las fuentes (Oswald, Open Sans) están **auto-hospedados** en `styles/` y `fonts/` a propósito — no introducir llamadas a Google Fonts ni CDNs.
- **Archivo (`archive/`).** 100 entradas en Markdown con nombre `YYYY-MM-DD-slug.md`. Cada fichero empieza con frontmatter:
  ```
  ---
  title: "..."
  date: YYYY-MM-DD
  tags: [tag1, tag2]
  source: https://desarrolla2.com/post/slug
  ---
  ```
  Respetar este formato al añadir o editar entradas.
- **Deploy automático.** Cualquier push a `main` publica en GitHub Pages vía `.github/workflows/deploy.yml`. Commitear con cuidado.

## Probar localmente

```bash
python3 -m http.server
```
