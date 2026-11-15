# Calculadora de automatización de procesos con IA

Calculadora interactiva que estima el impacto de automatizar un proceso empresarial con IA: potencial de automatización, horas ahorradas (semana/mes/año), ahorro económico, ROI y periodo de recuperación, y la herramienta recomendada (n8n, Make, Zapier, agentes de IA).

**🌐 Verla en línea:** https://asternodelab.github.io/calculadora-automatizacion-ia/

## Características

- Dashboard con gráficas interactivas construidas en SVG/CSS (sin librerías): dona de reparto del trabajo, comparación antes/después y curva de ahorro acumulado vs. inversión con punto de recuperación.
- Supuestos 100 % editables y transparentes: cada resultado muestra su fórmula con los números del usuario sustituidos. Nada de cifras "oficiales" inventadas.
- Recomendación de herramienta con scoring según volumen, tipo de datos y escala, más plan de piloto sugerido.
- Un solo archivo `index.html` — sin build, sin frameworks, funciona offline.

## Uso

Abrir `index.html` directamente en el navegador, o correr el servidor de desarrollo:

```bash
npm start   # → http://localhost:3000
```

Cambiar cualquier dato (horas, frecuencia, costo por hora, personas) recalcula todo al instante. En "Supuestos editables" se ajustan el potencial de automatización, la supervisión humana, la inversión, el costo mensual y la moneda.

## Publicación

El sitio se publica en GitHub Pages automáticamente con cada `git push` a `main`.

## Stack

HTML + CSS + JavaScript vanilla. Sin dependencias ni herramientas de compilación. Ver `CLAUDE.md` para la guía técnica completa (dirigida a agentes de código, útil también como documentación).
