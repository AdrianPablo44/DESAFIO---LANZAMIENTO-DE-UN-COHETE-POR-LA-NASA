# DESAFIO-LANZAMIENTO-DE-UN-COHETE-POR-LA-NASA
# Despegue de un cohete experimental, 1962

Aplicación web de simulación que calcula la velocidad y la altura de un cohete durante sus primeros segundos de ascenso. Aproxima la solución con el método de Euler y la compara con la solución analítica. Como ampliación, incorpora los métodos de Heun y Runge-Kutta de cuarto orden (RK4).

Es un único archivo HTML (`simulador_despegue_cohete.html`). No necesita instalación, servidor ni conexión a internet.

## Contexto

El desafío se inspira en el trabajo de las matemáticas de la NASA en la película *Talentos ocultos* (*Hidden Figures*). Se pide aproximar con Euler, paso a paso, el despegue de un cohete experimental y contrastar el resultado con la solución exacta, evaluando el error obtenido.

## Modelo matemático

```
m · dv/dt = T − m·g − k·v
dy/dt     = v
```

| Símbolo | Significado | Unidad |
|---|---|---|
| m | Masa del cohete | kg |
| T | Empuje (constante) | N |
| g | Aceleración de la gravedad | m/s² |
| k | Coeficiente de resistencia lineal del aire | kg/s |
| v | Velocidad vertical | m/s |
| y | Altura | m |

Al dividir entre m queda `v' = a − b·v`, con `a = T/m − g` y `b = k/m`.

### Solución analítica

```
v(t) = a/b + (v₀ − a/b) · e^(−b·t)
y(t) = y₀ + (a/b)·t + (v₀ − a/b) · (1 − e^(−b·t)) / b
```

Si `k = 0` (sin resistencia): `v(t) = v₀ + a·t` y `y(t) = y₀ + v₀·t + a·t²/2`.

### Métodos numéricos

Con paso `h` y el sistema `(v, y)`:

- **Euler:** `vₙ₊₁ = vₙ + h·(a − b·vₙ)` y `yₙ₊₁ = yₙ + h·vₙ`. Error global de orden 1.
- **Heun (Euler mejorado):** promedia la pendiente al inicio del paso y la pendiente predicha al final. Orden 2.
- **RK4:** combina cuatro evaluaciones de la derivada por paso. Orden 4.

## Cómo usarla

1. Abre `simulador_despegue_cohete.html` en cualquier navegador moderno (Chrome, Firefox, Edge, Safari).
2. Ajusta los parámetros o deja los valores por defecto.
3. Pulsa **Simular** (o Enter en cualquier campo).
4. Revisa los resultados en las secciones de la página.

### Parámetros por defecto

Son los que reproducen las cifras de la conclusión del enunciado. El enunciado no los da, se dedujeron de sus resultados. El botón **Valores del enunciado** los restaura.

| Parámetro | Valor |
|---|---|
| Masa m | 100 kg |
| Empuje T | 1500 N |
| Resistencia k | 10 kg/s |
| Gravedad g | 9,81 m/s² |
| Velocidad inicial v₀ | 0 m/s |
| Altura inicial y₀ | 0 m |
| Tiempo final | 20 s |
| Paso h | 2 s |

Con estos valores y h = 2 s, Euler obtiene v(20) = 46,3273 m/s e y(20) = 574,7272 m. La solución exacta da 44,8761 m/s y 589,2390 m.

### Validaciones

La aplicación avisa si falta algún dato, si la masa no es positiva, si k es negativa, si h es mayor que el tiempo final o si hay más de 5000 pasos. También advierte cuando el empuje no supera el peso (T ≤ m·g) y el cohete no despega.

## Qué muestra la aplicación

| Sección | Contenido |
|---|---|
| Modelo matemático | Ecuación diferencial y solución analítica con los coeficientes calculados y la velocidad límite. |
| Parámetros | Formulario de entrada y selección de métodos (Heun y RK4 son opcionales). |
| Simulación del despegue | Animación del cohete siguiendo la solución exacta, con la altura de Euler como punto de comparación. |
| Gráficas comparativas | Velocidad, altura y error absoluto en la velocidad frente al tiempo. |
| Tabla de iteraciones | Valores exactos y aproximados paso a paso, con error absoluto y relativo (%). |
| Efecto del tamaño de paso | Errores en el tiempo final para h = 2, 1, 0,5 y 0,25 s, con la justificación del método más preciso. |
| Preguntas a responder | Las 5 preguntas del enunciado, respondidas con los datos de la simulación actual. |
| Preguntas de reflexión | Las 5 preguntas de reflexión con una respuesta breve desplegable. |

## Correspondencia con la rúbrica

| Criterio | Dónde se cubre |
|---|---|
| Formulación e interpretación del modelo matemático | Sección *Modelo matemático* |
| Implementación correcta del método de Euler | Funciones `step` e `integrate` (método `euler`) |
| Solución analítica de velocidad y altura | Sección *Modelo matemático* y respuesta 3 |
| Tabla de iteraciones y comparación de errores | *Tabla de iteraciones* y *Efecto del tamaño de paso* |
| Gráficas y simulación web funcional | *Simulación del despegue* y *Gráficas comparativas* |
| Interpretación física y conclusiones | Respuestas 4 y 5, y las preguntas de reflexión |
| Presentación y explicación del trabajo | Este README y la propia página |

## Estructura del código

Todo está en un solo archivo, organizado así:

- **HTML y CSS:** estructura de la página y estilos, sin dependencias externas.
- **`exact(t, p)`:** solución analítica.
- **`step(met, s, h, f)`:** un paso de Euler, Heun o RK4.
- **`integrate(met, p, h, tEnd)`:** avanza en el tiempo con el método elegido. El último paso se acorta si `tEnd` no es múltiplo de `h`.
- **`renderTable`, `renderSteps`, `renderAnswers`:** generan las tablas y las respuestas.
- **`chart(...)`:** dibuja las gráficas directamente en `<canvas>`, sin librerías.
- **`drawRocket()`:** animación del cohete.

## Limitaciones del modelo

- Masa y empuje constantes, sin consumo de combustible.
- Resistencia del aire lineal en la velocidad. En la realidad es cuadrática y depende de la densidad del aire.
- Gravedad constante y movimiento puramente vertical.
- No se consideran viento, inclinación de la trayectoria ni rotación terrestre.

## Requisitos

Un navegador actual con soporte de HTML5 `<canvas>` y JavaScript habilitado.
