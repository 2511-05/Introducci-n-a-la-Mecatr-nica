# Práctica 5 — Actuadores: Control de Motores y Servomotores

## Objetivos
* Comprender e integrar actuadores electromecánicos fundamentales (Motores a Pasos, Servomotores y Motores DC) en sistemas mecatrónicos.
* Implementar etapas de potencia e interfaseado adecuado (Puente H L298N/L293D, controladores A4988/ULN2003 y drivers de servomotores).
* Analizar las técnicas de control de posición, velocidad y sentido de giro mediante señales PWM y secuencias de pasos.

---

## 1. Servomotor (Control de Posición Angular)

### Principio de Funcionamiento
Un servomotor integra un motor DC, una caja reductora y un potenciómetro de retroalimentación interna. La posición del eje se controla mediante una señal **PWM (Pulse-Width Modulation)** de $50\text{ Hz}$ (periodo de $20\text{ ms}$), donde el ancho del pulso (habitualmente entre $1\text{ ms}$ y $2\text{ ms}$) determina el ángulo de salida (de $0^\circ$ a $180^\circ$).

### Esquema y Simulación
![Esquema de conexión del Servomotor](../img_practica_5/servo_esquema.png)

*Figura 1: Conexión del servomotor alimentado externamente con señal de control en pin PWM.*

### Código de Implementación
```cpp
#include <Servo.h>

Servo miServo;
const int pinServo = 9;

void setup() {
  miServo.attach(pinServo);
}

void loop() {
  // Barrido de 0 a 180 grados
  for (int angulo = 0; angulo <= 180; angulo += 10) {
    miServo.write(angulo);
    delay(15);
  }
  delay(500);

  // Barrido de 180 a 0 grados
  for (int angulo = 180; angulo >= 0; angulo -= 10) {
    miServo.write(angulo);
    delay(15);
  }
  delay(500);
}

```

---

## 2. Motor DC con Puente H (Control de Velocidad y Sentido)

### Principio de Funcionamiento

Los motores de corriente continua requieren una etapa de potencia debido a que sus consumos de corriente superan los límites seguros del microcontrolador. Mediante un **Puente H (L298N o L293D)**, se invierte la polaridad aplicada al motor para cambiar el sentido de giro, y se aplica una señal **PWM** en los pines de habilitación (Enable) para modular la velocidad angular.

### Esquema y Simulación

*Figura 2: Interfaseado de Motor DC con driver Puente H y alimentación externa.*

### Código de Implementación

```cpp
const int in1 = 4;
const int in2 = 5;
const int ena = 6; // Pin PWM

void setup() {
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(ena, OUTPUT);
}

void loop() {
  // Giro en sentido horario a velocidad media (128/255)
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  analogWrite(ena, 128);
  delay(2000);

  // Giro en sentido antihorario a velocidad máxima (255/255)
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  analogWrite(ena, 255);
  delay(2000);

  // Paro de motor
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  analogWrite(ena, 0);
  delay(1000);
}

```

---

## 3. Motor a Pasos (Control de Pasos y Torque)

### Principio de Funcionamiento

Un motor a pasos (como el unipolar 28BYJ-48 con ULN2003 o un bipolar NEMA con A4988) divide una vuelta completa en un número exacto de pasos discretos. Energizando las bobinas internas en una secuencia determinada (Full-Step, Half-Step o Microstepping), se logra un control de posición preciso sin necesidad de sensores de retroalimentación externos.

### Esquema y Simulación

*Figura 3: Conexión de controlador de motor a pasos con secuencia de bobinado.*

### Código de Implementación

```cpp
#include <Stepper.h>

const int pasosPorVuelta = 200; // Ajustar según el motor
Stepper miStepper(pasosPorVuelta, 8, 9, 10, 11);

void setup() {
  miStepper.setSpeed(60); // 60 RPM
}

void loop() {
  // Una vuelta completa en sentido horario
  miStepper.step(pasosPorVuelta);
  delay(1000);

  // Una vuelta completa en sentido antihorario
  miStepper.step(-pasosPorVuelta);
  delay(1000);
}

```

---

## Análisis de Fallos y Diagnóstico

* **Reinicio inesperado del microcontrolador:** Causa común por picos de corriente o ruido inductivo de los motores. **Solución:** Separar la fuente de alimentación del circuito de control y compartir las tierras ($GND$).
* **Falta de torque en el servomotor o vibraciones:** Señal PWM inestable o voltaje de alimentación inferior al mínimo requerido ($5\text{V} - 6\text{V}$).
* **Calentamiento excesivo del puente H:** Inexistencia de diodos de libre circulación o sobrepaso de la corriente máxima continua nominal del CI.

---

## Aprendizaje

Comprendimos cómo la etapa de salida del ESP32 interactúa con dispositivos que requieren mayor potencia o precisión de movimiento. Aprendimos a estructurar secuencias lógicas de control mediante software y a garantizar la protección eléctrica del microcontrolador.

--- 

## Siguiente Paso

Integrar en un solo sistema los sensores leídos en prácticas anteriores (como el ultrasónico o LDR) para activar de forma automática y autónoma estos actuadores según el entorno.