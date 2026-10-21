# Proyecto 1 — Carrito Móvil Autónomo / Controlado

## Objetivos
- Diseñar e integrar un vehículo móvil autónomo/controlado utilizando la tarjeta microcontroladora.
- Dimensionar y fabricar un chasis funcional mediante iteraciones mecánicas para cumplir con las restricciones de tamaño de la competencia.
- Implementar la etapa de potencia e interfaz inalámbrica de comandos (vía Bluetooth) con control de tracción diferencial.
- Documentar las fallas estructurales y de acoplamiento sufridas durante el desarrollo y su respectiva resolución.

---

## 1. Requisitos del Proyecto

Para la validación y evaluación del vehículo se definieron los siguientes criterios de desempeño y construcción:

- **Dimensiones Límite:** Ajuste estricto del chasis al tamaño máximo estipulado por las reglas de la práctica.
- **Control Inalámbrico:** Recepción e interpretación de comandos mediante comunicación serie Bluetooth (HC-06).
- **Alimentación Autónoma:** Sistema de energía portátil e independiente para la electrónica de control y motores.
- **Acoplamiento Mecánico:** Ensamble sólido de motores, ruedas y componentes sin desprendimientos en movimiento.
- **Prueba de Rodamiento:** Desplazamiento continuo y estable en la pista de pruebas.

---

## 2. Planificación por Semanas

| Semana | Actividades Desarrolladas | Entregables / Hitos |
| :---: | :--- | :--- |
| **Semana 1** | Fabricación del primer prototipo de chasis (v1) y desarrollo de la lógica de código base para los motores. | Primer modelo físico y programa inicial de control. |
| **Semana 2** | Rediseño y fabricación del chasis definitivo (v2), montaje de componentes y pruebas de integración. | Ensamble final, corrección de tracción y prueba exitosa del carrito. |

### Procedimiento de Iteración

1. **Semana 1 — Prototipado Inicial y Lógica:**
   * Se diseñó el chasis v1 y se escribió el código de recepción de comandos vía Bluetooth.
   * *Resultado:* El prototipo funcionó electrónicamente, pero excedió las dimensiones límite del proyecto.

2. **Semana 2 — Rediseño Mecánico y Ensamble Final:**
   * Se construyó un nuevo chasis más sencillo con las dimensiones correctas para optimizar el espacio interior.
   * Se aplicó un pegamento más resistente en las llantas para asegurar su fijación y se realizó el ensamble definitivo con el driver TB6612FNG, superando las pruebas de rodamiento.

---

## 3. Lista de Materiales (BOM)

| Componente | Cantidad | Precio Unitario (MXN) | Precio Total (MXN) |
| :--- | :---: | :---: | :---: |
| **Tarjeta Microcontroladora** | 1 | $150.00 | $150.00 |
| **Módulo Bluetooth HC-06** | 1 | $80.00 | $80.00 |
| **Driver de Motores TB6612FNG** | 1 | $60.00 | $60.00 |
| **Batería Portátil (Power Bank)** | 1 | $200.00 | $200.00 |
| **Cables Jumpers (M-M / M-H)** | 1 Kit | $40.00 | $40.00 |
| **Motoreductores con Rueda** | 2 | $35.00 | $70.00 |
| **Switch / Interruptor ON-OFF** | 1 | $10.00 | $10.00 |
| **Material de Chasis v2** | 1 | $0.00 | $0.00 |
| **Pegamento de Alta Resistencia** | 1 | $25.00 | $25.00 |
| **TOTAL** | — | — | **$635.00** |

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
- **Descripción:** El primer prototipo del chasis (Semana 1) no cumplía con los límites de tamaño solicitados para la prueba.
- **Causa:** En el primer dibujo no se contempló el espacio adicional que ocupaban el ancho de las ruedas y la posición de la batería portátil.
- **Solución:** Realizar un nuevo chasis más sencillo y con las dimensiones correctas que permitiera ajustar todos los componentes dentro del límite permitido.

### Falla 2 — Desprendimiento continuo de las llantas
- **Descripción:** Durante los ensayos de movimiento, las ruedas se zafaban del eje de los motoreductores con facilidad al girar.
- **Causa:** Holgura en la unión mecánica entre la flecha del motor y el acople de la rueda ante el par de tracción y las vibraciones del movimiento.
- **Solución:** Utilizar un pegamento más resistente para fijar adecuadamente las llantas a los ejes de los motores, evitando que se desprendan durante el rodamiento.

---

---

## 6. Código de Implementación

El siguiente código escrito en C++ para Arduino utiliza la librería `SoftwareSerial` para la comunicación con el módulo Bluetooth HC-06 y controla las direcciones y velocidades de los motores mediante el puente H TB6612FNG:

```cpp
//TimeMec
#include <SoftwareSerial.h>

SoftwareSerial BT(0, 1);  // RX, TX en los pines por defecto

// Pines del TB6612FNG
#define PWMA 6
#define AIN1 7
#define AIN2 8
#define PWMB 11
#define BIN1 9
#define BIN2 10
#define STBY 12

void setup() {
    BT.begin(9600);      // Comunicación con HC-06

    // Configurar pines del TB6612FNG
    pinMode(PWMA, OUTPUT);
    pinMode(AIN1, OUTPUT);
    pinMode(AIN2, OUTPUT);
    pinMode(PWMB, OUTPUT);
    pinMode(BIN1, OUTPUT);
    pinMode(BIN2, OUTPUT);
    pinMode(STBY, OUTPUT);

    digitalWrite(STBY, HIGH);  // Activar el puente H
}

void mover(int velocidadA, int dirA1, int dirA2, int velocidadB, int dirB1, int dirB2) {
    analogWrite(PWMA, velocidadA);
    digitalWrite(AIN1, dirA1);
    digitalWrite(AIN2, dirA2);

    analogWrite(PWMB, velocidadB);
    digitalWrite(BIN1, dirB1);
    digitalWrite(BIN2, dirB2);
}

void loop() {
    if (BT.available()) {
        char command = BT.read();

        switch (command) {
            case 'U': // Adelante
                mover(140, HIGH, LOW, 150, HIGH, LOW);
                break;
            case 'D': // Atrás
                mover(120, LOW, HIGH, 150, LOW, HIGH);
                break;
            case 'L': // Izquierda
                mover(100, LOW, HIGH, 100, HIGH, LOW);
                break;
            case 'R': // Derecha
                mover(100, HIGH, LOW, 100, LOW, HIGH);
                break;
            case 'T': // Triángulo (Ejemplo: Acelerar)
                mover(195, HIGH, LOW, 205, HIGH, LOW);
                break;
            case 'Q': // Cuadrado (Ejemplo: Gira en el sitio a la izquierda)
                mover(160, LOW, HIGH, 150, LOW, HIGH);
                break;
            case 'C': // Círculo (Ejemplo: Gira en el sitio a la derecha)
                mover(150, HIGH, LOW, 160, HIGH, LOW);
                break;
            case 'X': // X (Detenerse)
                mover(0, LOW, LOW, 0, LOW, LOW);
                break;
        }
    }
}

```

## 7. Resultado Final y Demostración

### Fotografía del Carrito Terminado

![Resultado Final del Carrito](../img_proyecto_1/resultado_final.jpg)

*Figura 3: Ensamble final del carrito móvil sobre el chasis v2 definitivo.*

### Video de Demostración

<iframe width="100%" height="400" src="[https://www.youtube.com/embed/TU_ID_DE_YOUTUBE](https://www.youtube.com/embed/TU_ID_DE_YOUTUBE)" title="Demostración Proyecto 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

---

## 8. Lista de Requisitos Cumplidos

| Requisito Solicitado | Estado | Observación / Solución Aplicada |
| :--- | :---: | :--- |
| **Ajuste de Dimensiones** | ✔️ Cumplido | Fabricación de un nuevo chasis más sencillo con las dimensiones exactas solicitadas. |
| **Fijación Mecánica Firme** | ✔️ Cumplido | Uso de pegamento más resistente en el acople de las llantas para evitar desprendimientos. |
| **Alimentación Autónoma** | ✔️ Cumplido | Operación inalámbrica y continua mediante la batería portátil integrada en la estructura. |
| **Control Inalámbrico** | ✔️ Cumplido | Procesamiento correcto de comandos por Bluetooth con el módulo HC-06 y control con el driver TB6612FNG. |

---

## 9. Aprendizajes del Proyecto

1. **Diseño Estructural Simplificado:** Aprendimos que realizar un chasis con geometría más sencilla facilita el cumplimiento estricto de las dimensiones límite sin comprometer la capacidad de alojamiento de los componentes electrónicos.
2. **Fijación Mecánica y Adhesión:** Comprobamos la importancia de emplear un pegamento más resistente en las uniones críticas (como la flecha del motor con la llanta) para soportar las fuerzas de tracción y torque.
3. **Control Mecatrónico Integral:** Experimentamos la integración completa del driver TB6612FNG y la comunicación inalámbrica Bluetooth, coordinando la etapa de control y potencia de forma efectiva.