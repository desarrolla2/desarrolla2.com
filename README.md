# desarrolla2.com

Página de cierre de **Memorias de un desarrollador**, el blog personal de Daniel González (@desarrolla2), activo entre enero de 2010 y junio de 2019.

El blog original fue construido con PHP y Symfony y publicó 100 artículos sobre programación, Linux y vida profesional. En 2026 se tomó la decisión de cerrarlo. Esta página estática es su sustituto: un memorial que recoge la historia, las cifras y el stack técnico del proyecto.

## Archivo

Las 100 entradas del blog están preservadas en la carpeta [`archive/`](archive/), en formato Markdown, con el nombre `YYYY-MM-DD-slug.md`.

Cada fichero incluye un frontmatter con título, fecha, etiquetas y URL original, seguido del contenido completo del artículo.

## Estructura

```
.
├── index.html          # Página principal
├── 404.html            # Página de error (GitHub Pages)
├── images/             # Logo (PNG + WebP)
├── styles/             # Bootstrap 5.3 + estilos propios
├── fonts/              # Fuentes Oswald y Open Sans (auto-hospedadas)
├── archive/            # 100 entradas del blog en Markdown
├── robots.txt
├── humans.txt
├── sitemap.xml
└── .well-known/
    └── security.txt
```

## Stack

- HTML5 semántico
- CSS personalizado con custom properties
- [Bootstrap 5.3](https://getbootstrap.com/) (auto-hospedado)
- Oswald + Open Sans (auto-hospedadas, sin llamadas a Google en runtime)

No hay dependencias de build, ni npm, ni bundlers. Se abre directamente en el navegador.

## Uso

Abre `index.html` en cualquier navegador, o sírvela con cualquier servidor estático:

```bash
python3 -m http.server
```

El repositorio se publica automáticamente en GitHub Pages al hacer push a `main` mediante el workflow `.github/workflows/deploy.yml`.

## Autor

Daniel González — [linkedin.com/in/desarrolla2](https://www.linkedin.com/in/desarrolla2/) · [github.com/desarrolla2](https://github.com/desarrolla2) · [devtia.com](https://devtia.com/)

## Licencia

MIT — ver [LICENSE](LICENSE)
