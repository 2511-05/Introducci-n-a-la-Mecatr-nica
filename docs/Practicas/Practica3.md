# Sesión 3 — Puente H y control de sentido de giro de un motor DC

## Objetivos
* Armar un circuito en Puente H (mediante CI L293D / SN754410 o transistores) para controlar el sentido de giro de un motor DC 
* Integrar el control de velocidad por PWM previo con la inversión de giro 
* Comprobar la lógica de entradas de control y la protección contra cortocircuitos 

## Materiales
* (1×) Driver Puente H (L293D o SN754410)
* (1×) NE555 (DIP-8) para señal PWM
* (1×) Motor DC (5 V a 9 V)
* (2×) Pulsadores o interruptores (push buttons)
* (1×) Potenciómetro de 10 kΩ (para velocidad PWM)
* (1×) Fuente de alimentación (5 V / 9 V)
* Protoboard, cables de conexión, resistores varios

## Desarrollo

<img src="../img_practica_3/digital3.jpg" alt="Simulación del circuito en Tinkercad." width="50%">  
*Simulación del circuito en Tinkercad.*

[*Video demostración del cambio de giro y velocidad*](../img_practica_3/video-puente-h.mp4)

**Explicación:** El puente H permite cambiar la polaridad aplicada a las terminales del motor. Al activar la primera combinación de entradas, la corriente fluye en una dirección (giro a la derecha); al cambiar la combinación, la corriente fluye en sentido opuesto (giro a la izquierda). La señal PWM proveniente del 555 se conecta al pin *Enable* para controlar simultáneamente la velocidad.

| Entrada 1 (IN1) | Entrada 2 (IN2) | Pin Enable (PWM) | Estado del motor |
| --- | --- | --- | --- |
| Alto (5V) | Bajo (0V) | 100% Duty | Giro a la derecha (máxima velocidad) |
| Bajo (0V) | Alto (5V) | 100% Duty | Giro a la izquierda (máxima velocidad) |
| Alto (5V) | Bajo (0V) | 50% Duty | Giro a la derecha (velocidad media) |
| Bajo (0V) | Bajo (0V) | Cualquier valor | Detenido (freno pasivo) |
| Alto (5V) | Alto (5V) | Cualquier valor | Detenido (freno activo) |

## Fallas
* **Síntoma:** El integrado L293D se calentaba demasiado o el motor no giraba en una de las direcciones.
* **Cómo lo encontré:** Medimos los voltajes en los pines de entrada (IN1 e IN2) con el multímetro y descubrimos que ambas entradas estaban recibiendo señal alta al mismo tiempo por un mal cableado de los botones.
* **Solución:** Corregimos las conexiones de los pulsadores añadiendo resistores de *pull-down* (10 kΩ) a tierra para asegurar estados lógicos definidos en cero (0V) cuando los botones no estén presionados.

## Aprendizajes
Comprendimos la estructura interna y el funcionamiento del Puente H como etapa de potencia entre las señales de control de bajo voltaje y la carga inductiva del motor. Aprendimos a integrar el control de sentido de giro con el módulo de velocidad PWM sin causar cortocircuitos en la etapa de potencia.

## Siguiente paso
Integrar estos conceptos analógicos y de potencia en una tarjeta programable (como Arduino) para automatizar los movimientos del motor mediante código.