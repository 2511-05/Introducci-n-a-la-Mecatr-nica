# Sesión 4 — Lectura de sensores (LDR, Ultrasónico y Potenciómetro) con ESP32

## Objetivos
* Configurar e integrar múltiples sensores (analógicos y digitales) en el ESP32 
* Realizar lecturas analógicas (ADC) de un potenciómetro y un sensor de luz (LDR) 
* Medir distancias en centímetros utilizando un sensor ultrasónico HC-SR04 
* Procesar y mostrar los datos en tiempo real mediante el Monitor Serie 

## Materiales
* (1×) Módulo ESP32 (NodeMCU 32S o similar)
* (1×) Sensor Ultrasónico HC-SR04 (Trig / Echo)
* (1×) Fotorresistencia LDR (Sensor de luz) + resistor 10 kΩ (divisora de voltaje)
* (1×) Potenciómetro de 10 kΩ
* Protoboard, cables de conexión de distintos colores
* Cable de datos USB para conexión al equipo

## Desarrollo

<img src="../img_practica_4/esp32_sensores.jpg" alt="Circuito físico con ESP32, potenciómetro, LDR y sensor ultrasónico." width="50%">  
*Circuito armado en físico con el ESP32, potenciómetro, LDR y sensor ultrasónico en protoboard.*

<img src="../img_practica_4/digital4.jpg" alt="Simulación del circuito con sensores en Wokwi o Tinkercad." width="50%">  
*Simulación del circuito en plataforma digital.*

<img src="../img_practica_4/codigo_practica4.jpg" alt="Código fuente del programa en Arduino IDE." width="50%">  
*Código fuente implementado en Arduino IDE para la lectura de sensores.*

[*Video de demostración de lectura de sensores en tiempo real*](../img_practica_4/video-sensores-esp32.mp4)

**Explicación:** El ESP32 realiza tres tipos de lectura: las entradas analógicas (ADC) procesan la señal del potenciómetro y de la fotorresistencia LDR (convertidas de 0 a 4095). Por otro lado, mediante el sensor ultrasónico HC-SR04 se envía un pulso de 10 µs a través del pin *Trig* y se mide el tiempo de retorno de la onda reflejada mediante el pin *Echo*, calculando así la distancia en centímetros.

| Sensor / Componente | Condición de prueba | Lectura ADC / Salida | Interpretación |
| --- | --- | --- | --- |
| **Potenciómetro** | Extremo Mínimo / Extremo Máximo | 0 / 4095 | Rango completo de 0 a 3.3V |
| **LDR (Luz)** | Luz ambiental / Sensor cubierto | ~3000 / ~400 | Disminución de lectura analógica por falta de luz |
| **Ultrasónico** | Objeto cercano / Objeto lejano | 5.2 cm / 45.0 cm | Cálculo preciso en cm basado en el tiempo de vuelo del sonido |

## Fallas
* **Síntoma:** El sensor ultrasónico devolvía valores fijados en 0 cm o mediciones erráticas y saltos abruptos.
* **Cómo lo encontré:** Al abrir el Monitor Serie observamos que las mediciones de distancia no cambiaban ni al acercar la mano. Comprobamos con el multímetro los pines de alimentación.
* **Solución:** Descubrimos que el sensor HC-SR04 requiere una alimentación de 5V para funcionar con precisión, mientras que lo teníamos conectado a la salida de 3.3V del ESP32. Al conectarlo al pin `VIN` (5V de la fuente/USB), el sensor comenzó a medir correctamente.

## Aprendizajes
Comprendimos la diferencia práctica entre sensores con salidas analógicas continuas (LDR y potenciómetro) y sensores que requieren medición de pulsos de tiempo (ultrasónico HC-SR04). Además, aprendimos a manipular adecuadamente las librerías o fórmulas matemáticas para convertir unidades de tiempo en distancia (cm) y a interpretar múltiples entradas en simultáneo a través del Monitor Serie.

## Siguiente paso
Implementar condiciones lógicas que activen actuadores (como encender un zumbador/buzzer o un LED de advertencia) cuando la distancia detectada sea menor a un umbral específico o cuando la luz ambiental descienda.