# Sesión 5 — Control e integración de actuadores con ESP32

## Objetivos
* Integrar el control de múltiples actuadores (servomotores, motores DC o displays) mediante el ESP32 
* Implementar comunicación y control mediante señales de salida (PWM / Digitales) 
* Desarrollar lógica de programación para automatizar la secuencia de activación de los componentes 

## Materiales
* (1×) Módulo ESP32 (NodeMCU 32S o similar)
* (1×) Servomotor (SG90 o MG90S) / Motor DC
* (1×) Driver de potencia o módulo de control (Puente H L298N/L293D o Transistor)
* (1×) Fuente de alimentación externa (5V)
* Protoboard y cables de conexión

## Desarrollo

<img src="-" alt="Circuito físico de la Práctica 5 en protoboard." width="50%">  
*Circuito armado en físico sobre la protoboard con el ESP32 y actuadores.*

<img src="-" alt="Simulación digital del circuito." width="50%">  
*Simulación del circuito en plataforma digital.*

<img src="-" alt="Captura del código fuente en Arduino IDE." width="50%">  
*Código fuente implementado en Arduino IDE.*

[*Video de demostración del funcionamiento*](../img_practica_5/video-practica5.mp4)

**Explicación:** En esta práctica se configuró el ESP32 para controlar la respuesta de un sistema físico mediante señales de salida. Se emplearon canales de control PWM / digital para manipular la posición o velocidad del actuador en función de la lógica definida en el código de Arduino IDE, asegurando una etapa de potencia aislada para evitar sobrecargas en la tarjeta de desarrollo.

| Estado / Entrada | Señal de salida (ESP32) | Respuesta del actuador |
| --- | --- | --- |
| Estado Inicial / Reposo | 0% Duty / 0V | Desactivado / Posición 0° |
| Estado Intermedio | 50% Duty / Señal modulada | Actividad a media potencia / Posición 90° |
| Estado Máximo | 100% Duty / 3.3V | Máxima potencia / Posición 180° |

## Fallas
* **Síntoma:** El actuador no respondía o el ESP32 presentaba reinicios inesperados al activar el componente.
* **Cómo lo encontré:** Observamos caída de voltaje en la línea principal al intentar arrancar el actuador y revisamos las conexiones con el multímetro.
* **Solución:** Separamos la línea de alimentación del actuador conectándolo a una fuente externa de 5V y unificando únicamente las tierras (GND) con el ESP32.

## Aprendizajes
Comprendimos cómo la etapa de salida del ESP32 interactúa con dispositivos que requieren mayor potencia o precisión de movimiento. Aprendimos a estructurar secuencias lógicas de control mediante software y a garantizar la protección eléctrica del microcontrolador.

## Siguiente paso
Integrar en un solo sistema los sensores leídos en prácticas anteriores (como el ultrasónico o LDR) para activar de forma automática y autónoma estos actuadores según el entorno.