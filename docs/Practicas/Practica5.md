# Práctica 5: Comunicación Bluetooth y Protocolos de Control

---

## Objetivos
* **Enlace Bluetooth:** Configurar la comunicación serie con `SerialBT.begin()` (o `SoftwareSerial`), verificando que los comandos sean recibidos y mostrados correctamente en el Monitor Serial.
* **Emparejamiento Estable:** Verificar que la vinculación serie inalámbrica entre el dispositivo móvil (Smartphone) y el módulo sea estable antes de ejecutar acciones de control.
* **LED con Bluetooth:** Implementar el control digital de un LED mediante la recepción de comandos `ON` y `OFF` desde la aplicación del celular.
* **Limpieza de Cadenas:** Aplicar el método de depuración de cadenas (`mensaje.trim()`) para eliminar saltos de línea (`\r`, `\n`) o espacios en blanco que provoquen fallas en la comparación de cadenas.
* **Protocolo de Comandos:** Documentar e implementar la tabla de correspondencia entre comandos recibidos e instrucciones ejecutadas.

---

## Materiales y Componentes

| Componente | Cantidad | Descripción |
| :--- | :---: | :--- |
| **Microcontrolador** | 1 | Tarjeta de desarrollo (Arduino Uno, ESP32 o similar) |
| **Módulo Bluetooth** | 1 | HC-05 (Maestro/Esclavo) o HC-06 (Esclavo) |
| **Resistencias** | 2 | $1\text{ k}\Omega$ y $2.2\text{ k}\Omega$ (Divisor de tensión para la línea RX) |
| **Diodo LED** | 1 | LED indicador con resistencia limitadora de $220\Omega$ |
| **Dispositivo Móvil** | 1 | Smartphone Android con App Terminal Bluetooth (ej. *Serial Bluetooth Terminal*) |
| **Protoboard y Cables** | 1 | Cables Dupont macho-macho / macho-hembra |

---

## Protocolo de Comandos

Para garantizar un control preciso del sistema, se define la siguiente tabla de interpretación de comandos transmitidos vía Bluetooth:

| Comando Recibido | Acción Ejecutada | Respuesta enviada al Celular |
| :---: | :--- | :--- |
| `ON` | Enciende el LED conectado al pin digital | `LED ACTIVADO` |
| `OFF` | Apaga el LED conectado al pin digital | `LED DESACTIVADO` |

---

## 1. Configuración de Hardware y División de Voltaje

### Principio de Funcionamiento
El módulo Bluetooth se comunica con el microcontrolador mediante puertos **UART** ($TX$/$RX$). Dado que los pines de datos del módulo operan a $3.3\text{V}$, se instala un **divisor de tensión** en el pin de recepción ($RX$) para proteger el módulo frente a las señales de $5\text{V}$ del microcontrolador.

$$V_{RX} = V_{TX} \times \frac{R_2}{R_1 + R_2} = 5\text{V} \times \frac{2.2\text{ k}\Omega}{1\text{ k}\Omega + 2.2\text{ k}\Omega} \approx 3.43\text{V}$$

### Esquema de Conexión
![Esquema de conexión Bluetooth HC-05](../img_practica_5/bluetooth_esquema.png)

*Figura 1: Circuito de interfaz Bluetooth con acondicionamiento de señal en el pin RX.*

---

## 2. Código de Implementación

!!! tip "Uso del método .trim()"
    Las aplicaciones de terminal Bluetooth suelen enviar automáticamente caracteres invisibles de fin de línea (`\r` o `\n`). Al aplicar `mensaje.trim()`, eliminamos estos caracteres para que la comparación `mensaje == "ON"` o `mensaje == "OFF"` funcione de manera exacta.

=== "Arduino C++ (ESP32 - Bluetooth Nativo)"
    ```cpp
    #include "BluetoothSerial.h"

    BluetoothSerial SerialBT;
    const int pinLED = 2;

    void setup() {
      pinMode(pinLED, OUTPUT);
      digitalWrite(pinLED, LOW);

      Serial.begin(115200);
      
      // Inicialización del servicio Bluetooth
      SerialBT.begin("Mecatronica_BT"); 
      Serial.println("Bluetooth iniciado. Asóciate con 'Mecatronica_BT'");
    }

    void loop() {
      if (SerialBT.available()) {
        String mensaje = SerialBT.readString();
        
        // Limpieza de caracteres de escape y saltos de línea
        mensaje.trim(); 

        Serial.print("Comando recibido: [");
        Serial.print(mensaje);
        Serial.println("]");

        if (mensaje.equalsIgnoreCase("ON")) {
          digitalWrite(pinLED, HIGH);
          SerialBT.println("LED ACTIVADO");
        } else if (mensaje.equalsIgnoreCase("OFF")) {
          digitalWrite(pinLED, LOW);
          SerialBT.println("LED DESACTIVADO");
        }
      }
    }
    ```

=== "Arduino C++ (Arduino Uno - SoftwareSerial)"
    ```cpp
    #include <SoftwareSerial.h>

    SoftwareSerial miBT(2, 3); // RX = 2, TX = 3
    const int pinLED = 13;

    void setup() {
      pinMode(pinLED, OUTPUT);
      digitalWrite(pinLED, LOW);

      Serial.begin(9600);
      miBT.begin(9600);

      Serial.println("Bluetooth Listo. Esperando comandos ON / OFF...");
    }

    void loop() {
      if (miBT.available()) {
        String mensaje = miBT.readString();
        
        // Limpieza de caracteres \r y \n
        mensaje.trim(); 

        Serial.print("Comando interpretado: ");
        Serial.println(mensaje);

        if (mensaje.equalsIgnoreCase("ON")) {
          digitalWrite(pinLED, HIGH);
          miBT.println("LED ACTIVADO");
        } else if (mensaje.equalsIgnoreCase("OFF")) {
          digitalWrite(pinLED, LOW);
          miBT.println("LED DESACTIVADO");
        }
      }
    }
    ```

---

## Solución de Problemas y Diagnóstico

??? warning "El comando 'ON' o 'OFF' es enviado pero el LED no responde"
    **Causa:** La aplicación del celular transmite un salto de línea adicional (`\r\n`) al presionar enviar, haciendo que la cadena no coincida exactamente con `"ON"`.  
    **Solución:** Asegúrate de incluir `mensaje.trim()` antes de la instrucción de comparación `if`, o configura tu app móvil para que no envíe terminadores de línea (*New Line*).

??? warning "El emparejamiento se desconecta constantemente"
    **Causa:** Caídas de voltaje por fuente de alimentación inestable o interferencias en el pin $TX$/$RX$.  
    **Solución:** Verifica la solidez de las tierras ($GND$) compartidas y asegura que la señal en el pin $RX$ esté regulada con el divisor de tensión.

---

## Siguiente Paso

Una vez validado el protocolo de comandos inalámbricos básicos (`ON`/`OFF`), pasamos a la aplicación en sistemas dinámicos:

* **[Práctica 6: Integración de Sensores y Actuadores](../Practica6/)** — Control remoto de actuadores y lectura remota de variables analógicas.