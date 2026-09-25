# Sesión 2 — Control de velocidad de un motor DC mediante PWM con ESP32

## Objetivos
* Configurar los canales de salida PWM (LEDC) en un microcontrolador ESP32 
* Variar el duty cycle mediante un potenciómetro conectado a una entrada analógica (ADC) 
* Controlar la velocidad de un motor DC a través de un transistor y diodo flyback 

## Materiales
* (1×) Módulo ESP32 (NodeMCU 32S o similar)
* (1×) Potenciómetro de 10 kΩ
* (1×) Transistor NPN (TIP122, BD139 o 2N2222) + resistor de base (1 kΩ)
* (1×) Diodo 1N4007 (flyback para protección del transistor)
* (1×) Motor DC (5 V a 9 V)
* (1×) Fuente de alimentación externa para el motor
* Protoboard, cables de conexión

## Desarrollo

<img src="../img_practica_2/boton_rebote_esquema_p2.png" alt="Simulación del circuito en Wokwi o Tinkercad." width="50%">  
*Simulación del circuito del ESP32.*

[*Video de demostración del control de velocidad*](../img_practica_2/video-esp32-pwm.mp4)

**Explicación:** El ESP32 lee el valor analógico del potenciómetro (de 0 a 4095) a través de uno de sus pines ADC (ej. GPIO 34). Posteriormente, escala dicho valor a la resolución configurada del canal PWM (0 a 255 para 8 bits) y envía la señal por un pin de salida (ej. GPIO 18) hacia la base del transistor, ajustando la velocidad del motor.

### Código fuente (Arduino IDE)
```cpp
const int potPin = 34;   // Pin analógico conectado al potenciómetro
const int pwmPin = 18;   // Pin GPIO de salida PWM

// Configuración PWM del ESP32
const int freq = 5000;      // Frecuencia en Hz
const int pwmChannel = 0;   // Canal PWM (0 a 15)
const int resolution = 8;   // Resolución de 8 bits (0 - 255)

void setup() {
  // Configurar las propiedades del canal PWM
  ledcSetup(pwmChannel, freq, resolution);
  // Asignar el canal al pin GPIO
  ledcAttachPin(pwmPin, pwmChannel);
}

void loop() {
  int potValue = analogRead(potPin);                     // Leer valor (0 - 4095)
  int dutyCycle = map(potValue, 0, 4095, 0, 255);        // Escalar a 8 bits (0 - 255)
  
  ledcWrite(pwmChannel, dutyCycle);                      // Enviar señal PWM
  delay(15);
}