# desarrolla2.com

Página de cierre de **Memorias de un desarrollador**, el blog personal de Daniel González (@desarrolla2), activo entre enero de 2010 y junio de 2019.

El blog original fue construido con PHP y Symfony y publicó 100 artículos sobre programación, Linux y vida profesional. En 2025 se tomó la decisión de cerrarlo. Esta página estática es su sustituto: un memorial que recoge la historia, las cifras y el stack técnico del proyecto.

## Estructura

```
.
├── index.html   # Página principal
├── style.css    # Estilos
├── logo.png     # Logo de desarrolla2.com
└── README.md
```

## Stack

- HTML5 semántico
- CSS personalizado con custom properties
- [Bootstrap 5.3](https://getbootstrap.com/) via CDN (grid y utilidades)
- [Google Fonts](https://fonts.google.com/): Oswald + Open Sans

No hay dependencias de build, ni npm, ni bundlers. Se abre directamente en el navegador.

## Uso

Abre `index.html` en cualquier navegador, o sírvela con cualquier servidor estático:

```bash
npx serve .
# o
python3 -m http.server
```

## Autor

Daniel González — [linkedin.com/in/desarrolla2](https://www.linkedin.com/in/desarrolla2/) · [github.com/desarrolla2](https://github.com/desarrolla2) · [devtia.com](https://devtia.com/)

## Licencia

MIT — ver [LICENSE](LICENSE)
