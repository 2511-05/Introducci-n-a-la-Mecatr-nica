# Proyecto 1 — Carrito Móvil Autónomo / Controlado

## Objetivos
- Diseñar e integrar un vehículo móvil autónomo/controlado utilizando la tarjeta microcontroladora ESP32.
- Dimensionar y fabricar un chasis funcional mediante iteraciones mecánicas para cumplir con las restricciones de tamaño de la competencia.
- Implementar la etapa de potencia e interfaz inalámbrica de comandos (vía Bluetooth) con control de tracción diferencial.
- Documentar las fallas estructurales y de acoplamiento sufridas durante el desarrollo y su respectiva resolución.

---

## 1. Requisitos del Proyecto

Para la validación y evaluación del vehículo se definieron los siguientes criterios de desempeño y construcción:

* **Dimensiones Límite:** Ajuste estricto del chasis al tamaño máximo estipulado por las reglas de la práctica.
* **Control Inalámbrico:** Recepción e interpretación de comandos mediante comunicación serie Bluetooth en la ESP32.
* **Alimentación Autónoma:** Sistema de energía portátil e independiente (Power Bank) para la electrónica de control y motores.
* **Acoplamiento Mecánico:** Ensamble sólido de motores, ruedas y componentes sin desprendimientos en movimiento.
* **Prueba de Rodamiento:** Desplazamiento continuo y estable en la pista de pruebas.

---

## 2. Planificación por Semanas

| Semana | Actividades Desarrolladas | Entregables / Hitos |
| :---: | :--- | :--- |
| **Semana 1** | Fabricación del primer prototipo de chasis (v1) y desarrollo de la lógica de código base para los motores. | Primer modelo físico y programa inicial de control. |
| **Semana 2** | Rediseño y fabricación del chasis definitivo (v2), montaje de componentes y pruebas de integración. | Ensamble final, corrección de tracción y prueba exitosa del carrito. |

### Procedimiento de Iteración

1. **Semana 1 — Prototipado Inicial y Lógica:**
   * Se diseñó el chasis v1 y se escribió el código de recepción de comandos en la ESP32.
   * *Resultado:* El prototipo funcionó electrónicamente, pero excedió las dimensiones límite del proyecto.

2. **Semana 2 — Rediseño Mecánico y Ensamble Final:**
   * Se reconstruyó el chasis (versión v2) optimizando el espacio interior para alojar la batería portátil verticalmente.
   * Se realizó el ensamble definitivo del interruptor, motores y tarjeta ESP32, superando las pruebas de rodamiento.

---

## 3. Lista de Materiales (BOM)

| Componente | Cantidad | Precio Unitario (MXN) | Precio Total (MXN) |
| :--- | :---: | :---: | :---: |
| **Tarjeta ESP32** | 1 | $150.00 | $150.00 |
| **Batería Portátil (Power Bank)** | 1 | $200.00 | $200.00 |
| **Cables Jumpers (M-M / M-H)** | 1 Kit | $40.00 | $40.00 |
| **Motoreductores con Rueda** | 2 | $35.00 | $70.00 |
| **Switch / Interruptor ON-OFF** | 1 | $10.00 | $10.00 |
| **Material de Chasis v2** | 1 | $0.00 | $0.00 |
| **Otros Materiales / Ajustes** | 1 | $0.00 | $0.00 |
| **TOTAL** | — | — | **$470.00** |

---

## 4. Diseño Mecánico del Chasis

### Boceto y Modelo 3D

![Boceto del Chasis](../img_proyecto_1/boceto_chasis.png)

*Figura 1: Boceto conceptual del chasis y distribución espacial de la electrónica.*

![Modelo 3D del Chasis Final](../img_proyecto_1/modelo_3d.png)

*Figura 2: Modelado CAD 3D del prototipo final (Chasis v2).*

---

## 5. Bitácora de Fallas y Diagnóstico

### Falla 1 — Exceso en las dimensiones del chasis
* **Descripción:** El primer prototipo del chasis (Semana 1) no cumplía con los límites de tamaño solicitados para la prueba.
* **Causa:** En el primer dibujo no se contempló el espacio adicional que ocupaban el ancho de las ruedas y la posición de la batería portátil.
* **Solución:** Tuvimos que rehacer el chasis desde cero en la Semana 2, redistribuyendo los elementos de forma compacta para cumplir con las medidas exactas.

### Falla 2 — Desprendimiento continuo de las llantas
* **Descripción:** Durante los ensayos de movimiento, las ruedas se zafaban del eje de los motoreductores con facilidad.
* **Causa:** Holgura en la unión mecánica entre la flecha del motor y la rueda ante las vibraciones del giro.
* **Solución:** Se corrigió el acople en el chasis v2 aplicando mayor presión y alineación adecuada en los soportes de los motores.

---

## 6. Código de Implementación

Para asegurar una respuesta precisa a los comandos inalámbricos, se limpia la cadena recibida mediante `mensaje.trim()`, evitando fallos causados por caracteres de salto de línea (`\r`, `\n`).

```cpp
#include "BluetoothSerial.h"

BluetoothSerial SerialBT;

// Pines de control del puente H / motores
const int IN1 = 12;
const int IN2 = 13;
const int IN3 = 14;
const int IN4 = 27;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  Serial.begin(115200);
  SerialBT.begin("ESP32_Carrito_P1");
  Serial.println("Sistema listo. Esperando comandos via Bluetooth...");
}

void loop() {
  if (SerialBT.available()) {
    String mensaje = SerialBT.readString();
    
    // Limpieza de caracteres invisibles \r y \n
    mensaje.trim();

    Serial.print("Comando recibido: [");
    Serial.print(mensaje);
    Serial.println("]");

    if (mensaje.equalsIgnoreCase("ON") || mensaje.equalsIgnoreCase("F")) {
      // Avanzar
      digitalWrite(IN1, HIGH);
      digitalWrite(IN2, LOW);
      digitalWrite(IN3, HIGH);
      digitalWrite(IN4, LOW);
      SerialBT.println("Estado: Avanzando");
    } 
    else if (mensaje.equalsIgnoreCase("OFF") || mensaje.equalsIgnoreCase("S")) {
      // Detener
      digitalWrite(IN1, LOW);
      digitalWrite(IN2, LOW);
      digitalWrite(IN3, LOW);
      digitalWrite(IN4, LOW);
      SerialBT.println("Estado: Detenido");
    }
  }
}
---

## 7. Resultado Final y Demostración

### Fotografía del Carrito Terminado

![Resultado Final del Carrito](../img_proyecto_1/resultado_final.jpg)

*Figura 3: Ensamble final del carrito móvil sobre el chasis v2 definitivo.*

### Video de Demostración

<iframe width="100%" height="400" src="https://www.youtube.com/embed/TU_ID_DE_YOUTUBE" title="Demostración Proyecto 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

*Nota: Reemplaza `TU_ID_DE_YOUTUBE` por el identificador del video cargado.*

---

## 8. Lista de Requisitos Cumplidos

| Requisito Solicitado | Estado | Observación / Solución Aplicada |
| :--- | :---: | :--- |
| **Ajuste de Dimensiones** | ✔️ Cumplido | Rediseño exitoso en la Semana 2 mediante el chasis v2, adaptando la batería verticalmente para no sobrepasar los límites. |
| **Fijación Mecánica Firme** | ✔️ Cumplido | Corrección del acople entre las ruedas y los motoreductores, evitando que se desprendan durante la marcha. |
| **Alimentación Autónoma** | ✔️ Cumplido | Operación inalámbrica y continua mediante la batería portátil (Power Bank) integrada en la estructura. |
| **Control Inalámbrico** | ✔️ Cumplido | Procesamiento correcto de comandos `ON`/`OFF` y movimientos recibidos por comunicación Bluetooth en la ESP32. |

---

## 9. Aprendizajes del Proyecto

1. **Consideración de Volúmenes Reales en CAD:** Aprendimos a contemplar las dimensiones físicas absolutas de todos los periféricos (cables, conectores, llantas y batería) desde el primer boceto para evitar tener que rehacer el chasis por completo.
2. **Acondicionamiento Mecánico y Tolerancias:** Comprobamos la importancia de asegurar un acople firme entre las ruedas y la flecha de los motoreductores para resistir las vibraciones del movimiento continuo.
3. **Iteración de Prototipado:** Experimentamos el proceso real de desarrollo mecatrónico, donde las fallas detectadas en el primer prototipo sirvieron como base para lograr una versión v2 completamente funcional.
