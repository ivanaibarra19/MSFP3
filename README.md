<div align="center">

# 🦾 SISTEMA MUSCULOESQUELÉTICO
### Práctica 3 · Modelado y control

**Ingeniería Biomédica · Instituto Tecnológico de Tijuana**

![MATLAB](https://img.shields.io/badge/MATLAB-Simulink-orange) ![Python](https://img.shields.io/badge/Python-Modelado-blue) ![PID](https://img.shields.io/badge/Control-PID-green)

</div>

---

### Ficha académica

| Dato | Información |
|:--|:--|
| Alumna | María Luisa Michelle Muñoz Meneses |
| Número de control | 23210714 |
| Correo | l23210714@tectijuana.edu.mx |
| Asignatura | Modelado de Sistemas Fisiológicos |
| Docente | Dr. Paul Antonio Valle Trujillo |
| Institución | Tecnológico Nacional de México — IT Tijuana |

<div align="justify">

## 01 · Resumen

Se modela el sistema musculoesquelético con elementos contráctiles, elásticos y viscosos. A partir de la analogía fuerza–voltaje se obtiene un circuito equivalente y se comparan un individuo sano (control) y un caso simulado de atrofia muscular. Se propone un controlador PID para aproximar la respuesta del caso a la referencia sana mediante MATLAB, Simulink, Simscape Electrical y Python.

## 02 · Palabras clave

**Sistema musculoesquelético; modelo mecánico; analogía electromecánica; atrofia muscular; elasticidad; amortiguamiento; función de transferencia; PID; MATLAB; Python.**

## 03 · Objetivos

**General:** Analizar y controlar un modelo musculoesquelético comparando una condición sana con una condición de atrofia muscular simulada.

**Específicos:** identificar elementos mecánicos, obtener el circuito equivalente y su función de transferencia, evaluar estabilidad, simular las respuestas y sintonizar un controlador PID.

## 04 · Fundamento teórico

El sistema musculoesquelético genera movimiento mediante músculos y tendones. Su comportamiento puede aproximarse mediante un elemento contráctil, un amortiguador viscoso y dos elementos elásticos.

| Elemento | Símbolo | Representación |
|:--|:--:|:--|
| Fuerza activa | $F_0(t)$ | Contracción muscular |
| Fuerza aplicada | $F(t)$ | Excitación del sistema |
| Amortiguamiento | $R$ | Resistencia viscosa |
| Elasticidad paralela | $C_p$ | Componente elástica muscular |
| Elasticidad en serie | $C_s$ | Componente musculotendinosa |
| Desplazamientos | $x(t),x_1(t)$ | Movimiento total e interno |

La fuerza activa se define como:

$$
F_0(t)=\alpha F(t),\qquad \alpha=0.25
$$

## 05 · Analogía electromecánica

| Variable mecánica | Equivalente eléctrico |
|:--|:--|
| Fuerza | Voltaje |
| Velocidad | Corriente |
| Desplazamiento | Carga |
| Amortiguador | Resistencia |
| Compliancia | Capacitancia |
| Masa | Inductancia |

$$
F(t)\leftrightarrow V(t),\qquad \dot{x}(t)\leftrightarrow I(t),\qquad x(t)\leftrightarrow q(t)
$$

## 06 · Parámetros de simulación

| Parámetro | Control | Caso |
|:--|--:|--:|
| Entrada equivalente | 1 V | 1 V |
| $\alpha$ | 0.25 | 0.25 |
| $C_s$ | 10 µF | 10 µF |
| $C_p$ | 100 µF | 100 µF |
| $R$ | 100 Ω | 10 kΩ |

La resistencia equivalente del caso aumenta **100 veces** respecto al control; se trata de una representación simplificada, no de un parámetro clínico medido.

## 07 · Modelo matemático

La fuerza viscosa y las fuerzas elásticas se describen por:

$$
F_R(t)=R\frac{d}{dt}[x(t)-x_1(t)]
$$

$$
F_{C_p}(t)=\frac{x(t)}{C_p},\qquad F_{C_s}(t)=\frac{x_1(t)}{C_s}
$$

La función de transferencia buscada es:

$$
G(s)=\frac{X(s)}{F(s)}
$$

Se obtendrán $G_{\mathrm{control}}(s)$ y $G_{\mathrm{caso}}(s)$ a partir de las conexiones específicas del circuito equivalente. Los coeficientes definitivos quedan pendientes de verificar con el esquema.

## 08 · Estabilidad y error

Los polos se calculan a partir del denominador $D(s)$ de $G(s)=N(s)/D(s)$. La estabilidad asintótica requiere polos con parte real negativa. Cuando procede el teorema del valor final:

$$
x(\infty)=\lim_{s\to 0}sX(s)
$$

## 09 · Simulación y control

Se implementa el modelo mediante **ecuaciones en Simulink**, **bloque de función de transferencia** y **circuito en Simscape Electrical**, usando una entrada impulso y el intervalo de **0 a 10 s**.

En lazo abierto se comparan $x_{\mathrm{control}}(t)$ y $x_{\mathrm{caso}}(t)$. Para el control:

$$
e(t)=x_{\mathrm{control}}(t)-x_{\mathrm{caso}}(t)
$$

$$
K(s)=k_P+\frac{k_I}{s}+k_Ds
$$

Las ganancias se ajustan con **PID Tune**. En lazo cerrado se comparan las respuestas de control, caso y caso con PID.

| Ganancia | Valor obtenido |
|:--|:--|
| $k_P$ | Pendiente |
| $k_I$ | Pendiente |
| $k_D$ | Pendiente |

## 10 · Python y verificación

En Python se reproducen las funciones de transferencia, la respuesta al impulso y el sistema con PID. Se comparan los resultados con MATLAB, Simulink y Simscape, considerando las mismas condiciones iniciales y parámetros.

## 11 · BioRender

Se señalarán el **elemento contráctil**, la **elasticidad paralela**, la **elasticidad en serie asociada al tendón**, el **amortiguamiento**, la **fuerza** y el **desplazamiento** en una representación anatómica del músculo.

## 12 · Evidencias y resultados

- [ ] Circuito equivalente y desarrollo matemático.
- [ ] Simulación de control y caso en lazo abierto.
- [ ] Capturas de Simulink y Simscape Electrical.
- [ ] Ganancias PID y gráfica de lazo cerrado.
- [ ] Comparación de MATLAB y Python.
- [ ] Diagrama de BioRender.
- [ ] Archivos `README.md` y `CITATION.cff`.

| Indicador | Control | Caso | PID |
|:--|:--:|:--:|:--:|
| Amplitud máxima | Pendiente | Pendiente | Pendiente |
| Tiempo de establecimiento | Pendiente | Pendiente | Pendiente |
| Estabilidad | Pendiente | Pendiente | Pendiente |

## 13 · Conclusiones

La analogía electromecánica permite analizar la dinámica musculoesquelética. La comparación entre control y caso muestra el efecto de modificar el amortiguamiento equivalente; la eficacia del PID deberá establecerse con las gráficas y los valores obtenidos en simulación.

## 14 · Referencias

1. M. C. Khoo, *Physiological Control Systems*, 2.ª ed., IEEE Press, 2018.
2. F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.
3. J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14.ª ed., Elsevier, 2021.
4. P. A. Valle Trujillo, *Material de la asignatura Modelado de Sistemas Fisiológicos*, IT Tijuana.

</div>

---
<div align="center"><sub>Práctica 3 · Ingeniería Biomédica · Instituto Tecnológico de Tijuana</sub></div>
