# Sesión 4 — Lectura e interfaz de sensores con ESP32

## Objetivos
* Configurar entradas analógicas y digitales en el ESP32 para distintos tipos de sensores 
* Realizar la lectura de un potenciómetro para entender la conversión ADC 
* Medir la variación de iluminación mediante una fotorresistencia (LDR) 
* Medir la distancia en centímetros utilizando un sensor ultrasónico HC-SR04 

## Materiales
* (1×) Módulo ESP32 (NodeMCU 32S o similar)
* (1×) Sensor Ultrasónico HC-SR04
* (1×) Fotorresistencia LDR + resistor 10 kΩ
* (1×) Potenciómetro de 10 kΩ
* Protoboard, cables de conexión
* Cable USB para programación

## Desarrollo

### Parte 1: Lectura de Potenciómetro
<img src="-" width="50%">  
*Circuito del potenciómetro conectado al ESP32.*

<img src="-" width="50%">  
*Código fuente implementado para el potenciómetro.*

**Explicación:** Se conectó el potenciómetro a una entrada analógica (ADC) del ESP32. Al girar la perilla, el voltaje varía entre 0V y 3.3V, lo que el ESP32 convierte internamente en un rango numérico de 0 a 4095 que se imprime en el Monitor Serie.

---

### Parte 2: Sensor de Luz (LDR)
<img src="-" alt="Circuito de la fotorresistencia LDR." width="50%">  
*Circuito de la fotorresistencia LDR en protoboard.*

<img src="-" alt="Simulación de la LDR." width="50%">  
*Simulación de la lectura de luz con LDR.*

<img src="-" alt="Código para la LDR." width="50%">  
*Código fuente implementado para la fotorresistencia LDR.*

**Explicación:** La LDR cambia su resistencia según la cantidad de luz ambiental. En conjunto con una resistencia fija de 10 kΩ formando un divisor de voltaje, el ESP32 lee la variación de voltaje en su pin analógico: a mayor luz disponible, mayor es el valor devuelto por el ADC.

---

### Parte 3: Sensor Ultrasónico (HC-SR04)
<img src="-" alt="Circuito del sensor ultrasónico." width="50%">  
*Circuito del sensor ultrasónico conectado al ESP32.*

<img src="-" alt="Simulación del sensor ultrasónico." width="50%">  
*Simulación de la medición de distancia.*

<img src="-" alt="Código para el sensor ultrasónico." width="50%">  
*Código fuente implementado para el sensor ultrasónico.*

[*Video de demostración de los sensores*](../img_practica_4/video-sensores-esp32.mp4)

**Explicación:** Mediante el pin *Trig* se emite un pulso ultrasónico de alta frecuencia de 10 µs. Al chocar contra un objeto, el rebote regresa al pin *Echo*. El ESP32 mide el tiempo transcurrido con la función `pulseIn()` y calcula la distancia aplicando la fórmula de la velocidad del sonido.

---

| Sensor / Componente | Lectura en condición mínima | Lectura en condición máxima | Aplicación práctica |
| --- | --- | --- | --- |
| **Potenciómetro** | 0 (0 V) | 4095 (3.3 V) | Control manual de parámetros |
| **LDR (Luz)** | ~400 (Oscuridad) | ~3500 (Mucha luz) | Detección de iluminación / Noche |
| **Ultrasónico** | ~2.0 cm | ~200.0 cm | Detección de obstáculos y distancia |

## Fallas
* **Síntoma:** El sensor ultrasónico no medía nada o arrojaba 0 cm constantes en el Monitor Serie.
* **Cómo lo encontré:** Comprobamos que todo estuviera conectado a los pines correctos.
* **Solución:** Corregimos la conexión en el simulador ya que habiamos cometido un error al conectar al pin de 3.3V de la ESP32.

## Aprendizajes
Aprendimos a implementar e interpretar sensores de manera independiente. Comprendimos el funcionamiento de las entradas analógicas (ADC de 12 bits) para el potenciómetro y la LDR, y el manejo de temporización digital precisa mediante pulsos para medir distancia con el sensor ultrasónico.

## Siguiente paso
Combinar las lecturas individuales de estos sensores en un solo programa para tomar decisiones de control complejas (por ejemplo, encender un motor o activar una alarma cuando un objeto esté cerca o haya poca luz).