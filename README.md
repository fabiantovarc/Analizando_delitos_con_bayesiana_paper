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

## 🚀 Reproducibilidad rápida
```r
# 1) Instalar/activar renv
install.packages("renv"); renv::init()  # si es la primera vez
renv::restore()                          # para reproducir entornos

# 2) Renderizar sitio Quarto a /docs
install.packages(c("quarto", "rmarkdown", "brms", "tidyverse", "ggplot2"))
# Render local (verás la web en vivo)
# desde terminal: quarto preview

# 3) Renderizar el reporte (HTML)
rmarkdown::render("reportes/Proyecto_bayesiana_final.Rmd",
                 output_format = "html_document",
                 output_file = "../docs/reporte.html")
````

## 🖼️ Cómo se generan las figuras del README

Agrega `ggsave()` inmediatamente después de tus gráficos o usa `scripts/exportar_figuras.R`.

```r
# Ejemplo: posterior θ_adultos
p <- ggplot(df_post, aes(theta, dens)) +
  geom_line() +
  geom_vline(xintercept = 0.65, linetype = "dashed")

ggsave("images/posterior_adultos.png", p, width = 7, height = 4, dpi = 300)
```

Enlazadas en el README:

```markdown
![Posterior adultos](images/posterior_adultos.png)
![Tendencia λ Bogotá](images/lambda_bogota.png)
```

## 📘 Reporte y Paper

* **Reporte técnico**: `docs/reporte.html` (render del Rmd).
* **Paper**: `paper/` (PDF + bib) *(añádelo cuando lo tengas)*.

## 📣 Cómo citar

Ver `CITATION.cff` y agrega DOI (Zenodo) cuando publiques un release.

## 🔐 Datos y ética

* Datos agregados y anonimizados. Evita subir CSVs sensibles o muy pesados.
* Coloca datasets en `data/` y usa `.gitignore`; incluye un script para descargarlos.

## 📝 Licencia

MIT — libre uso con atribución. Ver [`LICENSE`](LICENSE).

---

### 👋 Autor

**Edisson Fabian Tovar Castro** — Ciencia de datos bayesiana aplicada a seguridad pública.
🌐 Portafolio / LinkedIn / Email (añade tus enlaces)

````

---

## ⚙️ _quarto.yml (sitio)
```yaml
project:
  type: website
  output-dir: docs

website:
  title: "Análisis Bayesiano de Delitos"
  navbar:
    left:
      - text: "Inicio"
        href: index.qmd
      - text: "Reporte técnico"
        href: reporte.html
    right:
      - icon: github
        href: https://github.com/TU_USUARIO/TU_REPO

format:
  html:
    theme: cosmo
    toc: true
    toc-depth: 3
    code-copy: true
    smooth-scroll: true

execute:
  freeze: auto   # evita recomputar en CI; usa caché local _freeze
````

---

## 🏠 index.qmd (landing que vende)

```markdown
---
title: "Análisis Bayesiano de Delitos: Bogotá vs. Cali"
description: "Modelos Bayesianos para decisiones de política pública — Beta–Binomial, Dirichlet–Multinomial y Poisson (2010–2019)."
image: images/hero_lambda_bogota.png
---

> **¿Qué problema resolví?** Traduzco datos de criminalidad en
> **probabilidades accionables** para priorizar intervenciones.

### Resultados rápidos

::: callout
#### Hipótesis 1 — arma
`P(θ > 0.5 | datos) ≈ 0.12` → **No** hay evidencia de que la mayoría sean sin armas.
:::

::: callout
#### Hipótesis 2 — adultos
`P(θ_adultos > 0.65 | datos) ≈ 0.83` → **Evidencia moderada**.
:::

::: callout
#### Tendencia 2010–2019
**Bogotá** con crecimiento marcado (regresión Poisson). **Cali**: crecimiento leve.
:::

### Gráficos clave

![](images/posterior_adultos.png)

![](images/lambda_bogota.png)

> El detalle metodológico completo está en el [reporte técnico](reporte.html).

### Metodología
- Conjugación Beta–Binomial (HPD 95%).
- Dirichlet–Multinomial y marginales Beta.
- `brms`/Stan para Poisson con enlace log.

### Reproducibilidad
- Entorno fijado con `renv`.
- Semillas (`set.seed`) y caché para Quarto (`freeze`).

### Sobre mí
**Edisson Fabian Tovar Castro** — Científico de datos (Bayes), enfoque en seguridad pública.
```

---

## 🧰 scripts/exportar_figuras.R

```r
# Ejecuta este script DESPUÉS de correr tus chunks que crean objetos de figura.
# Ajusta nombres de data.frames/plots a los de tu Rmd.

# 1) Posterior adultos (sustituye `df_post` / `a_adultos` si tus nombres varían)
if (exists("df_post")) {
  p1 <- ggplot(df_post, aes(theta, dens)) +
    geom_line() +
    geom_vline(xintercept = 0.65, linetype = "dashed") +
    labs(title = "Posterior de theta_adultos")
  dir.create("images", showWarnings = FALSE)
  ggsave("images/posterior_adultos.png", p1, width = 7, height = 4, dpi = 300)
}

# 2) Evolución lambda Bogotá (si tienes `lambda_df`)
if (exists("lambda_df")) {
  p2 <- ggplot(lambda_df, aes(year, mean)) +
    geom_ribbon(aes(ymin = q025, ymax = q975), alpha = 0.3) +
    geom_line() + geom_point() +
    labs(title = "Evolución de la tasa esperada — Bogotá", y = expression(lambda[t]))
  ggsave("images/lambda_bogota.png", p2, width = 7, height = 4, dpi = 300)
}

# 3) (opcional) Cualquier otra figura clave…
```

---

## 🧪 Ajustes mínimos a tu Rmd (`reportes/Proyecto_bayesiana_final.Rmd`)

> Añade al YAML del Rmd para también generar HTML (además de PDF):

```yaml
output:
  html_document:
    toc: true
    toc_float: true
    number_sections: true
  pdf_document:
    latex_engine: xelatex
```

> Y al final del Rmd, llama a `source("scripts/exportar_figuras.R")` para exportar imágenes.

---

## 🙈 .gitignore (R + datos)

```gitignore
.Rproj.user
.Rhistory
.RData
.Ruserdata

# renv cache local
renv/library/
renv/staging/

# datos crudos pesados
/data/raw/
*.csv

# cachés de Quarto
_freeze/
.quarto/

# docs generados
/docs/
/images/
```

> **Nota**: si versionas `docs/` y `images/` para GitHub Pages, quítalos del `.gitignore`.

---

## 🧾 CITATION.cff

```yaml
cff-version: 1.2.0
title: "Análisis Bayesiano de Delitos: Bogotá vs. Cali (2010–2019)"
authors:
  - family-names: Tovar Castro
    given-names: Edisson Fabian
keywords: [Bayes, brms, Poisson, Dirichlet, seguridad pública, R]
license: MIT
repository-code: https://github.com/TU_USUARIO/TU_REPO
abstract: >-
  Modelos Bayesianos (Beta–Binomial, Dirichlet–Multinomial y Poisson con brms)
  para analizar tendencias de criminalidad y apoyar decisiones de política pública.
```

---

## 📜 LICENSE (MIT)

```text
MIT License

Copyright (c) 2025 Edisson Fabian Tovar Castro

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🛠️ (Opcional) CI/CD con GitHub Actions para Quarto

> Recomendado si **no** recompilas `brms` en CI. Con `execute: freeze: auto`, el sitio usa caché.

`.github/workflows/quarto.yml`

```yaml
name: Build site
on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: r-lib/actions/setup-r@v2
      - uses: quarto-dev/quarto-actions/setup@v2
      - name: Instalar deps R mínimas
        uses: r-lib/actions/setup-r-dependencies@v2
        with:
          packages: |
            any::rmarkdown
            any::tidyverse
            any::ggplot2
      - name: Render sitio (usa freeze)
        run: quarto render
      - name: Subir artefacto (docs)
        uses: actions/upload-pages-artifact@v3
        with:
          path: docs
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

> Activa **Settings → Pages → Source: GitHub Actions** o, si prefieres sin CI, elige **/docs** como fuente y sube `docs/` directamente.

---

## 💼 Branding express (para el About del repo)

* **Tagline**: “Probabilidades accionables para seguridad pública con Bayes y `brms`”.
* **Social preview**: `images/social-card.png` (gráfico + tu nombre/cargo).
* **SEO**: Bayes, brms, Dirichlet, Poisson, HPD, seguridad, Colombia, Bogotá, Cali, R.
* **Call to action**: “¿Necesitas priorizar intervenciones o evaluar políticas? Abro DM/Email.”

---

## ✅ Checklist de implementación

1. Copia **README.md**, **_quarto.yml**, **index.qmd**, **CITATION.cff**, **LICENSE**.
2. Ajusta rutas en tu Rmd y añade `source("scripts/exportar_figuras.R")` para exportar imágenes.
3. Renderiza local: `quarto render` y `rmarkdown::render(...)` → se llena `docs/`.
4. Activa GitHub Pages (desde `docs/` o vía Actions).
5. Sube el **paper** a `paper/` y enlázalo en README.
6. (Luego) Crea un **release** y conecta **Zenodo** para DOI.

```
```
