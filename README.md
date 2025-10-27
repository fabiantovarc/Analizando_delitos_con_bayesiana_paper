# Análisis Bayesiano de Tasas de Criminalidad en Colombia (2010-2019)
# 1. Primer Modelo: Proporción de Delitos Sin Armas (Binomial-Beta)

## 1. Introducción

Breve descripción del problema: queremos saber si más del 50 % de los atracos se cometen sin armas.

## 2. Hipótesis

-   **Hipótesis nula ($H_0$)**
    La proporción de delitos sin armas es igual o menor al 50 %.
    $$H_0: \theta \le 0.5$$

-   **Hipótesis alternativa ($H_1$)**
    La proporción de delitos sin armas supera el 50 %.
    $$H_1: \theta > 0.5$$

## 3. Modelo de Verosimilitud (Binomial)

Modelamos el número de delitos sin armas ($y$) como una distribución binomial, dado un número total de delitos ($N$) y una proporción desconocida ($\theta$):
$$y | \theta \sim \text{Binomial}(N, \theta)$$

La verosimilitud proporcional es:
$$L(\theta | y) \propto \theta^y \cdot (1 - \theta)^{(N - y)}$$

## 4. Modelo Posterior (Conjugada Beta)

Usando una prior conjugada $\text{Beta}(\alpha, \beta)$, la distribución posterior de $\theta$ es:
$$\theta \mid y \sim \text{Beta}(\alpha + y,\ \beta + N - y)$$

## 5. Resultados y Conclusión

### 5.1. Probabilidad Posterior

La probabilidad posterior de que la hipótesis alternativa sea cierta es:
$$Pr(\theta > 0.5 | \text{datos}) = 0.1195$$

Esto significa que, dado el modelo y los datos observados, existe solo una **probabilidad posterior del 11.95\%** de que **más de la mitad de los delitos se cometan sin armas**.

**Interpretación**: Este valor no es suficientemente alto como para rechazar la hipótesis nula. **La evidencia respalda $H_0$**.

### 5.2. Intervalo Creíble al 95\%

$$IC_{95\%}(\theta) = [0.4227, 0.5192]$$

Este intervalo indica que, con un 95\% de credibilidad, la proporción de delitos sin armas se encuentra entre **42.27\% y 51.92\%**.

**Interpretación**: Como el valor 0.5 está contenido dentro del intervalo, **no se puede descartar la hipótesis nula**.

### 5.3. Conclusión Final (Modelo 1)

Con base en la distribución posterior y su intervalo creíble al 95\%, **no se encuentra evidencia estadística fuerte a favor de la hipótesis alternativa**. Por tanto, **no se puede concluir** que la mayoría de los delitos sean cometidos sin armas.

---

# 2. Segundo Modelo: Perfil Etario de Víctimas (Multinomial-Dirichlet)

## 1. Introducción

Queremos saber:
> ¿Es la proporción de delitos cometidos contra adultos ($\theta_{\text{adultos}}$) superior al 65 %?

### Hipótesis Bayesiana

-   **Hipótesis nula ($H_0$):**
    La proporción de víctimas adultas es igual o menor al 65\%.
    $$H_0 : \theta_{\text{adultos}} \le 0.65$$

-   **Hipótesis alternativa ($H_1$):**
    La proporción de víctimas adultas supera el 65\%.
    $$H_1 : \theta_{\text{adultos}} > 0.65$$

## 2. Modelo y Verosimilitud (Multinomial)

Los conteos por grupo etario $\mathbf{y} = (y_1, y_2, y_3, y_4)$ siguen una distribución **multinomial**:
$$\mathbf{y} \mid \boldsymbol{\theta} \sim \text{Multinomial}(N, \boldsymbol{\theta})$$

La verosimilitud es:
$$\mathcal{L}(\boldsymbol{\theta} \mid \mathbf{y}) \propto \prod_{k=1}^{4} \theta_k^{y_k}$$

## 3. Modelo Posterior (Conjugada Dirichlet)

Usando una prior conjugada $\text{Dirichlet}(\boldsymbol{\alpha})$, la posterior también es una Dirichlet con parámetros actualizados:
$$\boldsymbol{\theta} \mid \mathbf{y} \sim \text{Dirichlet}(\boldsymbol{\alpha} + \mathbf{y})$$

## 4. Resultados y Conclusión

### 4.1. Probabilidad Posterior

La probabilidad posterior de que la proporción de adultos supere el 65% es:
$$P(\theta_{\text{adultos}} > 0.65 \mid \text{datos}) = 0.8262$$

Esto indica que hay un **82.62\%** de probabilidad de que la proporción real de delitos cometidos contra adultos supere el 65 \%. Este valor denota **evidencia moderada** a favor de la hipótesis alternativa.

### 4.2. Intervalo Creíble al 95\%

La marginal de una Dirichlet es una Beta. El intervalo creíble al 95\% para $\theta_{\text{adultos}}$ es:
$$IC_{95\%}(\theta_{\text{adultos}}) = [0.6283, 0.7092]$$

**Interpretación**: El intervalo creíble se encuentra entre **62.83\%** y **70.92\%**. Como el límite inferior está por debajo de 0.65, la **evidencia no es concluyente** bajo un umbral de alta exigencia.

### 4.3. Conclusión Final (Modelo 2)

No se cumple el criterio de decisión bayesiana para considerar **evidencia fuerte**. Aunque la probabilidad posterior es alta (82.62\%), el intervalo creíble del 95\% no se encuentra completamente por encima de 0.65.

Por tanto, **no hay evidencia suficiente para afirmar que más del 65\% de los delitos fueron cometidos contra adultos**.

---

# 3. Tercer Modelo: Evolución Temporal de Delitos (Regresión Poisson)

## 3.1. Análisis de Bogotá

### 3.1.1. Pregunta e Hipótesis

¿La tasa anual de delitos en Bogotá ($\lambda_{\text{Bog},t}$) ha aumentado entre 2010 y 2019?
Modelamos el conteo $Y_t$ como:
$$Y_{\text{Bog},t} \mid \lambda_{\text{Bog},t} \sim \text{Poisson}(\lambda_{\text{Bog},t})$$
$$\log \lambda_{\text{Bog},t} = \alpha_{\text{Bog}} + \beta_{\text{Bog}} \cdot (t - 2010)$$

-   **$H_0$:** La tasa no ha aumentado ($\beta_{\text{Bog}} \le 0$).
-   **$H_1$:** La tasa ha aumentado ($\beta_{\text{Bog}} > 0$).

### 3.1.2. Resultados y Conclusión (Bogotá)

El análisis posterior (vía MCMC con `brms` y *sampler* manual) arroja:
-   $P(\beta_{\text{Bog}} > 0 \mid \text{datos}) \approx 1.000$
-   $P(\lambda_{\text{Bog},2019} > \lambda_{\text{Bog},2010} \mid \text{datos}) \approx 1.000$
-   $IC_{95\%}$ para el ratio $\lambda_{2019} / \lambda_{2010}$: **[15.95, 24.85]**

**Conclusión (Bogotá):** Existe **evidencia contundente** de que la tasa de delitos en Bogotá ha aumentado de manera sostenida. El modelo estima que la tasa de 2019 fue, con un 95\% de credibilidad, entre 16 y 25 veces mayor que la tasa de 2010.

## 3.2. Análisis de Cali

### 3.2.1. Pregunta e Hipótesis

¿La tasa anual de delitos en Cali ($\lambda_{\text{Cali},t}$) ha aumentado entre 2010 y 2019?
-   **$H_0$:** La tasa no ha aumentado ($\beta_{\text{Cali}} \le 0$).
-   **$H_1$:** La tasa ha aumentado ($\beta_{\text{Cali}} > 0$).

### 3.2.2. Resultados y Conclusión (Cali)

-   $P(\beta_{\text{Cali}} > 0 \mid \text{datos}) \approx 0.7555$
-   $P(\lambda_{\text{Cali},2019} > \lambda_{\text{Cali},2010} \mid \text{datos}) \approx 0.7555$
-   $IC_{95\%}$ para el ratio $\lambda_{2019} / \lambda_{2010}$: **[0.818, 1.510]**

**Observación:** La probabilidad posterior de crecimiento es moderada (75.55\%). El intervalo creíble para el ratio de tasas es amplio e **incluye el 1.0**, lo que significa que no se puede descartar la hipótesis nula (estabilidad o incluso un leve descenso).

**Conclusión (Cali):** No hay evidencia estadística fuerte para afirmar que la tasa de delitos en Cali haya aumentado. Los datos son consistentes con una tendencia estable.

## 3.3. Comparación: Bogotá vs. Cali

### 3.3.1. Pregunta e Hipótesis

¿Ha aumentado más rápido la tasa de delitos en Bogotá que en Cali?
-   **$H_0$:** El crecimiento de Bogotá es menor o igual al de Cali ($\beta^{\text{Bogotá}} \le \beta^{\text{Cali}}$).
-   **$H_1$:** El crecimiento de Bogotá es mayor que el de Cali ($\beta^{\text{Bogotá}} > \beta^{\text{Cali}}$).

### 3.3.2. Resultados y Conclusión (Comparativa)

Comparamos las muestras posteriores de las pendientes $\Delta = \beta^{\text{Bogotá}} - \beta^{\text{Cali}}$:
-   $P(\beta^{\text{Bogotá}} > \beta^{\text{Cali}} \mid \text{datos}) \approx 1.000$
-   $IC_{95\%}$ para la diferencia de pendientes ($\Delta$): **[0.28, 0.36]**

**Conclusión Final (Comparativa):** La inferencia bayesiana indica con una probabilidad posterior de prácticamente el 100\% que la pendiente de crecimiento anual es **mayor en Bogotá que en Cali**. Dado que el intervalo creíble para la diferencia de pendientes `[0.28, 0.36]` está completamente por encima de cero, la evidencia es concluyente.

La tasa esperada de delitos se ha incrementado mucho más rápido en Bogotá que en Cali durante el período 2010‑2019.
