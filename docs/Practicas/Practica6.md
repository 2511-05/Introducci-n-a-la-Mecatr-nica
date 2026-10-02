# Hoja de Ejercicios — Mecanismos y Cinemática

## Objetivos
* Analizar y calcular las relaciones de transmisión en trenes de engranes simples, compuestos y mecanismos de transmisión ($i$) 
* Determinar las velocidades angulares ($\omega$) y pares mecánicos ($\tau$) de salida en sistemas reductores 
* Calcular la cinemática lineal y diferencial de una plataforma móvil sobre ruedas 

---

## Ejercicio 1 — Tren simple

**Enunciado:** Un piñón de 10 dientes mueve un engrane de 40 dientes. El motor entrega 300 rpm y $0.1\text{ N}\cdot\text{m}$. ¿A qué velocidad y con qué par gira la salida? (Ignorar pérdidas por fricción.)

### Procedimiento

1. **Identificamos los datos:**
   * Dientes del piñón (entrada): $Z_1 = 10$
   * Dientes del engrane (salida): $Z_2 = 40$
   * Velocidad de entrada: $\omega_{\text{entrada}} = 300\text{ rpm}$
   * Par de entrada: $\tau_{\text{entrada}} = 0.1\text{ N}\cdot\text{m}$

2. **Calculamos la relación de transmisión ($i$):**
   $$i = \frac{Z_2}{Z_1} = \frac{40}{10} = 4$$

   > **Nota:** Como $i > 1$, se trata de un sistema reductor (la salida gira más lento pero entrega más par).

3. **Velocidad de salida ($\omega_{\text{salida}}$):**
   $$\omega_{\text{salida}} = \frac{\omega_{\text{entrada}}}{i} = \frac{300\text{ rpm}}{4} = 75\text{ rpm}$$

4. **Par de salida ($\tau_{\text{salida}}$):**
   $$\tau_{\text{salida}} = \tau_{\text{entrada}} \times i = 0.1\text{ N}\cdot\text{m} \times 4 = 0.4\text{ N}\cdot\text{m}$$

| Parámetro | Entrada | Salida |
| --- | --- | --- |
| **Número de Dientes** | $Z_1 = 10$ | $Z_2 = 40$ |
| **Velocidad Angular** | 300 rpm | **75 rpm** |
| **Par Mecánico (Torque)** | $0.1\text{ N}\cdot\text{m}$ | **$0.4\text{ N}\cdot\text{m}$** |

**Resultado:** La salida gira a **75 rpm** con un par de **$0.4\text{ N}\cdot\text{m}$**.

---

## Ejercicio 2 — Tren compuesto

**Enunciado:** Dos etapas en serie: 12-36 dientes, seguida de 10-40 dientes. ¿Cuál es la relación total? Si la entrada gira a 960 rpm, ¿a qué velocidad gira la salida final?

### Procedimiento

En un tren compuesto con varias etapas en serie, las relaciones individuales se multiplican:

1. **Relación de la primera etapa ($i_1$):**
   $$i_1 = \frac{36}{12} = 3$$

2. **Relación de la segunda etapa ($i_2$):**
   $$i_2 = \frac{40}{10} = 4$$

3. **Relación de transmisión total ($i_{\text{total}}$):**
   $$i_{\text{total}} = i_1 \times i_2 = 3 \times 4 = 12 \quad \text{(Relación 1:12)}$$

4. **Velocidad de la salida final ($\omega_{\text{salida}}$):**
   $$\omega_{\text{salida}} = \frac{\omega_{\text{entrada}}}{i_{\text{total}}} = \frac{960\text{ rpm}}{12} = 80\text{ rpm}$$

**Resultado:** La relación total es **12 (1:12)** y la salida final gira a **80 rpm**.

---

## Ejercicio 3 — Sinfín

**Enunciado:** Un sinfín de 2 hilos mueve una corona de 40 dientes. (En un sinfín, $Z_1$ es el número de hilos.) ¿Cuál es la relación de transmisión? ¿Cuántas vueltas del sinfín se necesitan para una vuelta de la corona?

### Procedimiento

1. **Identificamos los datos:**
   * Número de hilos del sinfín ($Z_1$): 2
   * Dientes de la corona ($Z_2$): 40

2. **Cálculo de la relación de transmisión ($i$):**
   $$i = \frac{Z_2}{Z_1} = \frac{40}{2} = 20$$

> **Interpretación:** Una relación de $i = 20$ (1:20) indica que por cada vuelta completa de la corona, el sinfín debe realizar 20 vueltas completas.

**Resultado:** La relación de transmisión es **20 (1:20)** y se requieren **20 vueltas del sinfín** por cada vuelta de la corona.

---

## Ejercicio 4 — Cruz de Ginebra

**Enunciado:** Contar las ranuras de la cruz del laboratorio y calcular: grados que avanza por cada paso, y vueltas completas del impulsor necesarias para una vuelta completa de la cruz.

### Procedimiento

Tomando como referencia estándar un modelo de laboratorio de $n = 4$ ranuras:

1. **Grados que avanza por cada paso:**
   $$\text{Grados por paso} = \frac{360^\circ}{n} = \frac{360^\circ}{4} = 90^\circ$$

2. **Vueltas del impulsor necesarias:**
   En una cruz de Ginebra de 4 ranuras, el disco impulsor realiza 1 vuelta completa por cada avance (paso de $90^\circ$) de la cruz. Por lo tanto, para dar 1 vuelta completa a la cruz ($360^\circ$), el impulsor requiere dar **4 vueltas completas**.

**Resultado:** Avanza **$90^\circ$ por paso** y se requieren **4 vueltas del impulsor** para 1 vuelta completa de la cruz.

---

## Ejercicio 5 — Velocidad del carro

**Enunciado:** El motor TT tiene reducción interna 1:48 y, a 6 V, la rueda gira aproximadamente 200 rpm sin carga. Con ruedas de 65 mm de diámetro, usando $v = \pi \cdot D \cdot \frac{\text{rpm}}{60}$, ¿cuál es la velocidad máxima teórica del carro en m/s? ¿Por qué en el piso real será menor que ese valor teórico?

### Procedimiento

1. **Identificamos los datos:**
   * Velocidad de la rueda: $200\text{ rpm}$
   * Diámetro de la rueda ($D$): $65\text{ mm} = 0.065\text{ m}$

2. **Cálculo de la velocidad teórica ($v$):**
   $$v = \pi \cdot D \cdot \frac{\text{rpm}}{60}$$
   $$v = \pi \cdot 0.065\text{ m} \cdot \frac{200}{60}$$
   $$v = \pi \cdot 0.065 \cdot 3.3333 \approx 0.6806\text{ m/s}$$

### Análisis del entorno real
La velocidad sobre el suelo real es menor al valor teórico debido a los siguientes factores:
* **Fricción mecánica:** Pérdidas por rozamiento en los ejes y dientes de los engranes.
* **Deslizamiento (patinaje):** Pérdida de tracción entre el neumático y la superficie.
* **Carga útil y par:** El dato de 200 rpm corresponde a un estado *sin carga*; al soportar el peso de la estructura, el motor demanda mayor torque, reduciendo sus RPM efectivas.

**Resultado:** La velocidad máxima teórica es de **$0.68\text{ m/s}$**.

---

## Ejercicio 6 — Dirección diferencial

**Enunciado:** La rueda izquierda va a $0.4\text{ m/s}$, la derecha a $0.6\text{ m/s}$, y la separación entre ruedas es $L = 0.12\text{ m}$. Usando las ecuaciones cinemáticas diferenciales, calcular la velocidad del centro del carro, su velocidad de giro, y el radio de la curva que describe.

### Procedimiento

1. **Velocidad lineal del centro del carro ($v$):**
   $$v = \frac{v_{\text{der}} + v_{\text{izq}}}{2} = \frac{0.6 + 0.4}{2} = \frac{1.0}{2} = 0.5\text{ m/s}$$

2. **Velocidad angular / de giro ($\omega$):**
   $$\omega = \frac{v_{\text{der}} - v_{\text{izq}}}{L} = \frac{0.6 - 0.4}{0.12} = \frac{0.2}{0.12} \approx 1.6667\text{ rad/s}$$

3. **Radio de curvatura del marco ($R$):**
   $$R = \frac{v}{\omega} = \frac{0.5\text{ m/s}}{1.6667\text{ rad/s}} = 0.3\text{ m}$$

| Variable Cinemática | Ecuación | Resultado |
| --- | --- | --- |
| **Velocidad del centro ($v$)** | $\frac{v_{\text{der}} + v_{\text{izq}}}{2}$ | **$0.5\text{ m/s}$** |
| **Velocidad de giro ($\omega$)** | $\frac{v_{\text{der}} - v_{\text{izq}}}{L}$ | **$1.66\text{ rad/s}$** |
| **Radio de la curva ($R$)** | $\frac{v}{\omega}$ | **$0.3\text{ m}$ (30 cm)** |

**Resultado:** Velocidad central de **$0.5\text{ m/s}$**, velocidad de giro de **$1.66\text{ rad/s}$** y radio de giro de **$0.3\text{ m}$**.

---

## Ejercicio 7 — Diseño e integración de caja reductora

**Enunciado:** Se busca que el carro sea el doble de "fuerte" para empujar la pelota en el torneo, aceptando ir a la mitad de velocidad. Proponer una relación de engranes adicional entre motor y rueda, y calcular la nueva velocidad máxima resultante.

### Procedimiento

1. **Propuesta de relación de engranes ($i_{\text{adicional}}$):**
   Para duplicar el par de salida y reducir la velocidad lineal a la mitad, se requiere incorporar una etapa de reducción adicional $2:1$ ($i = 2$).
   
   * *Ejemplo de diseño:* Un piñón conductor de 15 dientes ($Z_1 = 15$) acoplado a un engrane conducido de 30 dientes ($Z_2 = 30$).
     $$i_{\text{adicional}} = \frac{30}{15} = 2$$

2. **Nueva velocidad máxima resultante ($v_{\text{nueva}}$):**
   Tomando la velocidad inicial teórica calculada en el Ejercicio 5 ($v \approx 0.6806\text{ m/s}$):
   $$\text{Nueva velocidad} = \frac{v_{\text{anterior}}}{i_{\text{adicional}}} = \frac{0.6806\text{ m/s}}{2} = 0.3403\text{ m/s}$$

**Resultado:** Se propone una relación de reducción de **2:1** (ejemplo: 15 a 30 dientes), resultando en un incremento al doble en la fuerza de empuje y una nueva velocidad máxima de aproximadamente **$0.34\text{ m/s}$**.