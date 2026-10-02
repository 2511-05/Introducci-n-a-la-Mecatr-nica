Aquí tienes el código **Markdown completo** para la **Práctica 4**, conservando todo el texto detallado de la práctica y utilizando la sintaxis nativa de imágenes `![Texto](../Ruta/imagen.ext)`:

```markdown
# Práctica 4 — Sensores Analógicos Individuales

## Objetivos
* Interfazar y caracterizar de manera independiente tres tipos de sensores analógicos habituales: Potenciómetro, Fotorresistor (LDR) y Sensor Ultrasónico (HC-SR04).
* Adquirir y acondicionar señales analógicas mediante las entradas del Convertidor Analógico-Digital (ADC) del microcontrolador/tarjeta de desarrollo.
* Validar el comportamiento, rango de medición y respuesta de cada sensor mediante esquemas, simulación e implementación física.

---

## 1. Potenciómetro (Divisor de Tensión Variable)

### Principio de Funcionamiento
El potenciómetro actúa como un divisor de tensión ajustable manualmente. Al girar su perilla, se modifica la proporción de resistencia entre el terminal central (wiper) y los extremos conectados a $V_{CC}$ (5V) y $GND$. Esto produce un voltaje de salida analógico continuo que varía proporcionalmente entre $0\text{ V}$ y $5\text{ V}$.

### Esquema y Simulación
![Circuito y Simulación del Potenciómetro](../img_practica_4/potenciometro_esquema.png)

*Figura 1: Diagrama de conexión y simulación del potenciómetro conectado a una entrada analógica.*

### Código de Implementación
```cpp
const int potPin = A0;
int valorADC = 0;
float voltaje = 0.0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  valorADC = analogRead(potPin);
  voltaje = (valorADC * 5.0) / 1023.0;
  
  Serial.print("Lectura ADC: ");
  Serial.print(valorADC);
  Serial.print(" | Voltaje: ");
  Serial.print(voltaje);
  Serial.println(" V");
  
  delay(200);
}

```

---

## 2. Sensor de Luz (Fotorresistor LDR)

### Principio de Funcionamiento

La fotorresistencia (LDR) varía su resistencia eléctrica en función de la intensidad de luz incidente: a mayor iluminación, menor resistencia. Para convertir esta variación de resistencia en un voltaje medible por el ADC, se configura un circuito divisor de tensión con una resistencia fija de pull-down ($10\text{ k}\Omega$).

### Esquema y Simulación

*Figura 2: Configuración en divisor de voltaje para el sensor de luz LDR.*

### Código de Implementación

```cpp
const int ldrPin = A1;
int valorLDR = 0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  valorLDR = analogRead(ldrPin);
  
  Serial.print("Nivel de luz (Valor ADC): ");
  Serial.println(valorLDR);
  
  delay(300);
}

```

---

## 3. Sensor Ultrasónico (HC-SR04)

### Principio de Funcionamiento

El HC-SR04 mide distancias emitiendo ráfagas de ultrasonido a $40\text{ kHz}$ mediante el pin `Trig`. Al rebotar en un objeto, el eco regresa y es detectado por el sensor, manteniendo el pin `Echo` en nivel alto durante el tiempo transcurrido ($t$). Conociendo la velocidad del sonido en el aire ($\approx 343\text{ m/s}$ o $0.0343\text{ cm/}\mu\text{s}$), se calcula la distancia mediante la fórmula:

$$\text{Distancia (cm)} = \frac{t \times 0.0343}{2}$$

### Esquema y Simulación

*Figura 3: Conexión de los pines de disparo (Trig) y eco (Echo) del sensor ultrasónico.*

### Código de Implementación

```cpp
const int trigPin = 9;
const int echoPin = 8;

long duracion;
float distancia;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duracion = pulseIn(echoPin, HIGH);
  distancia = duracion * 0.0343 / 2.0;
  
  Serial.print("Distancia calculada: ");
  Serial.print(distancia);
  Serial.println(" cm");
  
  delay(250);
}

```

---

## Análisis de Resultados y Conclusiones

* **Potenciómetro:** Presenta una respuesta lineal y muy estable, ideal para ajustes de calibración o control directo de variables por parte del usuario.
* **LDR:** Muestra una respuesta no lineal pero altamente sensible a variaciones de luz ambiental, requiriendo umbrales de software para aplicaciones tipo encendido/apagado.
* **HC-SR04:** Proporciona mediciones precisas de distancia en un rango de $2\text{ cm}$ a $400\text{ cm}$, requiriendo filtrado por software en caso de detectar ecos falsos o rebotes en ángulos inclinados.
