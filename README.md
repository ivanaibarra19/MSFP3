[![Open in MATLAB Online](https://www.mathworks.com/images/responsive/global/open-in-matlab-online.svg)](https://matlab.mathworks.com/)

# Práctica 3: Modelado y control del sistema musculoesquelético

## Información de la estudiante

Nombres y Apellidos [No. Control]; correo institucional

Modelado de Sistemas Fisiológicos

Ingeniería Biomédica

## Docente

Dr. Paul Antonio Valle Trujillo; paul.valle@tectijuana.edu.mx

Departamento de Ingeniería Eléctrica y Electrónica, Tecnológico Nacional de México/IT Tijuana, Blvd. Alberto Limón Padilla s/n, Tijuana, C.P. 22454, B.C., México.

## Descripción de la asignatura

La asignatura Modelado de Sistemas Fisiológicos utiliza herramientas matemáticas y computacionales para estudiar cómo responden distintos sistemas del organismo ante una entrada. Su contenido reúne conocimientos de fisiología, mecánica, circuitos eléctricos y teoría de control, de utilidad para la formación en Ingeniería Biomédica.

En esta actividad se estudia el comportamiento mecánico del músculo mediante un modelo equivalente que combina elasticidad y amortiguamiento. Las simulaciones en MATLAB, Simulink, Simscape Electrical y Python permiten estudiar una condición de referencia y un caso de atrofia muscular, sin realizar pruebas clínicas.

## Objetivos

1. Reconocer las fuerzas, deformaciones y componentes mecánicos que intervienen en el modelo.
2. Relacionar los elementos musculoesqueléticos con un circuito eléctrico por medio de la analogía fuerza–voltaje.
3. Plantear las ecuaciones del circuito y resolverlas utilizando transformada de Laplace y superposición.
4. Obtener las funciones de transferencia para las condiciones de control y atrofia muscular.
5. Examinar los polos, la estabilidad y el comportamiento en estado estacionario.
6. Obtener la respuesta al impulso de los modelos en el intervalo de 0 a 10 segundos.
7. Implementar las representaciones matemáticas y eléctricas en MATLAB/Simulink y Simscape Electrical.
8. Incorporar un controlador PID al caso para aproximar su respuesta a la referencia sana.
9. Comprobar las respuestas mediante Python y documentar los resultados de las simulaciones.
10. Relacionar los elementos del modelo con estructuras anatómicas en un diagrama biológico.

## Descripción detallada del sistema

El sistema musculoesquelético se representa mediante una fuerza muscular activa y tres elementos pasivos: un amortiguador viscoso R y dos compliancias, Cp y Cs. La fuerza generada por la contracción se identifica como F0(t), mientras que F(t) describe la fuerza transmitida. Los desplazamientos x(t) y x1(t) permiten establecer las relaciones entre los elementos.

La fuerza contráctil se expresa como F0(t) = α F(t), donde α se encuentra entre 0 y 1. El circuito eléctrico equivalente se obtiene mediante la analogía fuerza–voltaje; para establecer correctamente sus conexiones es necesario considerar que la disposición en serie y paralelo puede cambiar al pasar del sistema mecánico al eléctrico.

### Correspondencia mecánica y eléctrica

| Sistema mecánico | Sistema eléctrico equivalente |
|---|---|
| Fuerza F(t) | Voltaje V(t) |
| Velocidad dx(t)/dt | Corriente I(t) |
| Desplazamiento x(t) | Carga eléctrica q(t) |
| Amortiguador | Resistencia R |
| Compliancia mecánica | Capacitancia C |
| Masa | Inductancia L |

En esta analogía, las conexiones en serie y en paralelo del modelo mecánico pueden intercambiarse al construir el circuito equivalente, por lo que es importante identificar los desplazamientos y las referencias antes de conectarlo.

### Valores utilizados en los dos modelos

| Parámetro | Control (sano) | Caso (atrofia muscular) |
|---|---:|---:|
| F(t) | 1 V | 1 V |
| α | 0.25 | 0.25 |
| Cs | 10 µF | 10 µF |
| Cp | 100 µF | 100 µF |
| R | 100 Ω | 10 kΩ |

El parámetro que cambia entre ambas condiciones es la resistencia R: el caso presenta una resistencia equivalente mayor. La salida del individuo sano se toma como referencia y la diferencia de desplazamientos se define mediante:

$$
e(t)=x_{control}(t)-x_{caso}(t)
$$

### Desarrollo de las funciones de transferencia

Se aplica superposición para estudiar individualmente el efecto de cada fuente. La salida Fs(s) representa la fuerza obtenida en el circuito equivalente. La transferencia resultante relaciona fuerzas; la transferencia de desplazamiento X(s)/F(s) requiere identificar y desarrollar por separado la salida de desplazamiento.

**1. Respuesta debida a F(t), con F0(t) apagada**

La relación de las variables del circuito es:

$$
x(t)=x_1(t)+x_2(t)
$$

$$
x(t)=C_p\frac{dF_{s1}(t)}{dt}
$$

$$
x_1(t)=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}
$$

$$
x_2(t)=\frac{F(t)-F_{s1}(t)}{R}
$$

Sustituyendo:

$$
C_p\frac{dF_{s1}(t)}{dt}=C_s\frac{d[F(t)-F_{s1}(t)]}{dt}+\frac{F(t)-F_{s1}(t)}{R}
$$

Aplicando la transformada de Laplace con condiciones iniciales nulas:

$$
C_p sF_{s1}(s)=C_s s[F(s)-F_{s1}(s)]+\frac{F(s)-F_{s1}(s)}{R}
$$

Agrupando términos:

$$
\left[(C_p+C_s)s+\frac{1}{R}\right]F_{s1}(s)=\left(C_s s+\frac{1}{R}\right)F(s)
$$

La primera contribución queda:

$$
\boxed{\frac{F_{s1}(s)}{F(s)}=\frac{RC_s s+1}{R(C_p+C_s)s+1}}
$$

**2. Respuesta debida a F0(t), con F(t) apagada**

Para la contribución de la fuente contráctil, con la convención de signos del circuito:

$$
-\alpha F(t)=R x(t)+\frac{1}{C_p+C_s}\int x(t)\,dt
$$

En el dominio de Laplace:

$$
-\alpha F(s)=\left[R+\frac{1}{(C_p+C_s)s}\right]X(s)
$$

Despejando:

$$
X(s)=\frac{-\alpha(C_p+C_s)s}{R(C_p+C_s)s+1}F(s)
$$

Como la contribución de salida es:

$$
F_{s2}(s)=\frac{X(s)}{(C_p+C_s)s}
$$

se obtiene:

$$
\boxed{\frac{F_{s2}(s)}{F(s)}=\frac{-\alpha}{R(C_p+C_s)s+1}}
$$

**3. Suma de las contribuciones**

Sumando las dos contribuciones:

$$
F_s(s)=F_{s1}(s)+F_{s2}(s)
$$

$$
F_s(s)=\frac{RC_s s+1}{R(C_p+C_s)s+1}F(s)-\frac{\alpha}{R(C_p+C_s)s+1}F(s)
$$

Por lo tanto:

$$
\boxed{H(s)=\frac{F_s(s)}{F(s)}=\frac{RC_s s+1-\alpha}{R(C_p+C_s)s+1}}
$$

**4. Modelos numéricos**

Para el individuo sano, R = 100 Ω:

$$
\boxed{H_{control}(s)=\frac{0.001s+0.75}{0.011s+1}}
$$

Para el caso de atrofia muscular, R = 10 000 Ω:

$$
\boxed{H_{caso}(s)=\frac{0.1s+0.75}{1.1s+1}}
$$

**5. Polos, estabilidad y valor final**

Las funciones de transferencia obtenidas son de primer orden, con un polo real cada una:

$$
p_{control}=-\frac{1}{0.011}\approx-90.91
$$

$$
p_{caso}=-\frac{1}{1.1}\approx-0.9091
$$

Ambos polos son negativos, por lo que los dos modelos de transferencia de fuerza son estables. El caso presenta una dinámica más lenta debido al incremento de R. Las constantes de tiempo correspondientes son 0.011 s para el control y 1.1 s para el caso.

Ante una entrada escalón unitario, el valor final de ambas salidas de fuerza es:

$$
F_s(\infty)=H(0)=1-\alpha=0.75
$$

Por lo tanto, el error estacionario entre ambas respuestas de fuerza es cero, aunque sus respuestas transitorias son diferentes. Si el error se define entre la entrada unitaria y la salida de fuerza, es 1 − 0.75 = 0.25. Estos resultados no sustituyen el análisis de error del desplazamiento solicitado para el sistema de control.

### Simulaciones del sistema

Se obtiene la respuesta al impulso unitario en el intervalo de 0 a 10 segundos utilizando MATLAB/Simulink, Simscape Electrical y Python. Se presentan las gráficas correspondientes a cada condición y se documentan los resultados obtenidos.

El caso se conecta a un controlador PID, cuyas ganancias se ajustan con **Tune** de Simulink para disminuir la diferencia con el control. Se deben guardar los valores utilizados y la respuesta resultante.

El diagrama biológico debe señalar el músculo contráctil, el tendón, los componentes elásticos y viscosos, la fuerza y el desplazamiento, indicando su correspondencia con el circuito.

**Palabras clave:** Sistema musculoesquelético; Atrofia muscular; Analogía fuerza–voltaje; Superposición; Función de transferencia; Controlador PID; MATLAB; Python.

## Lista de archivos incluidos en el repositorio

1. Archivo de MATLAB Live Script [.mlx] y versión exportada [.pdf].
2. Modelos de Simulink y Simscape Electrical [.slx].
3. Programa de simulación en Python [.py].
4. Desarrollo matemático del circuito, con análisis por superposición y funciones de transferencia.
5. Gráficas de las respuestas del individuo sano, del caso y del sistema controlado [.pdf y .png].
6. Registro de las ganancias del PID y capturas de su configuración.
7. Diagrama biológico y evidencias de simulación.
8. Archivos de documentación README.md y CITATION.cff.

## Referencias

[1] P. A. Valle, *Syllabus para Modelado de Sistemas Fisiológicos*, Tecnológico Nacional de México / Instituto Tecnológico de Tijuana, Tijuana, B.C., México, 2026. https://biomath.xyz/course/

[2] M. C. Khoo, *Physiological Control Systems: Analysis, Simulation, and Estimation*, 2nd ed., IEEE Press, 2018.

[3] F. C. Hoppensteadt y C. S. Peskin, *Modeling and Simulation in Medicine and the Life Sciences*, Springer.

[4] J. E. Hall, *Guyton and Hall Textbook of Medical Physiology*, 14th ed., Elsevier, 2021.

[5] K. Ogata, *Ingeniería de Control Moderna*, 5.ª ed., Pearson Educación, 2010.

[6] N. S. Nise, *Control Systems Engineering*, 8th ed., John Wiley & Sons, 2020.
