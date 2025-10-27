# Analizando_delitos_con_bayesiana_paper
```markdown
# Análisis Bayesiano de Delitos: Bogotá vs. Cali (2010–2019)

[![Made with R](https://img.shields.io/badge/Made%20with-R-276DC3.svg)](#)
[![brms/Stan](https://img.shields.io/badge/Bayesian-brms%2FStan-8A2BE2)](#)
[![Reproducible-yes](https://img.shields.io/badge/Reproducible-yes-brightgreen)](#)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![DOI](https://img.shields.io/badge/DOI-Zenodo-blue)](#)

> **Elevator pitch**. Modelé fenómenos de criminalidad con métodos Bayesianos (Beta–Binomial, Dirichlet–Multinomial, y regresión Poisson con `brms`) para responder preguntas de política pública. El proyecto prioriza **reproducibilidad** y **comunicación clara** con visualizaciones y sitio web.

## 🔎 Qué vas a encontrar
- **Hipótesis 1 (armas)**: probabilidad de que >50% de los atracos sean sin armas.
- **Hipótesis 2 (edad)**: probabilidad de que >65% de los delitos sean cometidos por **adultos**.
- **Serie temporal**: tendencia 2010–2019 con **regresión Poisson** (Bogotá y Cali) y comparación de pendientes.

## 🎯 Hallazgos (resumen ejecutivo)
- `P(θ > 0.5 | datos)` (sin armas): **≈ 0.12**
- `P(θ_adultos > 0.65 | datos)`: **≈ 0.83**
- Tendencia 2010–2019: **Bogotá ↑** (razón λ_2019/λ_2010 >> 1) vs **Cali** (↑ leve).

> Los valores se recalculan desde el código; ver detalles en el **Reporte** y el **sitio**.

## 🌐 Demo del proyecto (GitHub Pages)
- Sitio: **https://TU_USUARIO.github.io/TU_REPO/**  
- Reporte técnico (HTML): `docs/reporte.html`  
- Figuras clave: `images/`

## 🧠 Stack metodológico
- **Beta–Binomial** y **HPD** para proporciones.
- **Dirichlet–Multinomial** para categorías (grupos etarios).
- **Poisson** con enlace log (`brms`) para series anuales.

## 🗂️ Estructura del repo
```

.
├── README.md
├── _quarto.yml
├── index.qmd                 # Landing del sitio
├── docs/                     # Salida del sitio (GitHub Pages)
├── images/                   # Gráficos exportados para el README/sitio
├── reportes/
│   └── Proyecto_bayesiana_final.Rmd  # Tu Rmd original (ajustado)
├── scripts/
│   └── exportar_figuras.R    # Guardado de gráficos clave
├── renv.lock
├── .Rprofile
├── .gitignore
├── CITATION.cff
└── LICENSE

````
