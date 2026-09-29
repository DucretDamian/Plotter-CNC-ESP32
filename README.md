# 📌 UTN FRCU – Tecnologías para la Automatización ESP32 2026

## 👥 Team
- **Team number:** 5
- **Members:**
  - Ducret Leturia Damian Osvaldo
  - Leiva Octavio
  - Meier Carlos David
  - Schenberger Valentina
  - Szajnowicz Ignacio

---

## 🤖 Project Description
- **Description:** Diseño, desarrollo e implementación de un Plotter CNC (Control Numérico por Computadora) en 2D. El sistema es capaz de interpretar coordenadas generadas digitalmente (G-Code) en una computadora y traducirlas en movimientos físicos sincronizados en un plano cartesiano (ejes X, Y, Z) para trazar gráficos en una superficie usando un instrumento de dibujo (birome o lápiz).
<!-- [Brief description of the project's purpose] -->

- **Technology used:** ESP32, Arduino IDE, C/C++, Librerías de Arduino (AccelStepper, MultiStepper, WebSocketsServer, ArduinoJson, Preferences), HTML/WebSockets (para la interfaz), G-Code, Inkscape (vectorización), jscut (generador G-code), UGS (Universal Gcode Sender).
<!-- [Example: ESP32, Arduino IDE, Wokwi] -->

---

## 🔩 Components Used
<!--
| Component | Quantity | Notes |
|---|---|---|
| ESP32 DevKit | [ ] | [e.g. ESP32-WROOM-32] |
| [Sensor/Actuator] | [ ] | [e.g. DHT22, ultrasonic, LED, servo] |
| [Component] | [ ] | [ ] |
| [Component] | [ ] | [ ] |
| Breadboard / Jumper wires | [ ] | [ ] |
| Power supply | [ ] | [e.g. USB / external 5V] |
-->
| Component | Quantity | Notes |
|---|---|---|
| ESP32 DevKit | 1 | Microcontrolador principal (Opera lógica a 3.3V, provee conectividad WiFi/Bluetooth) |
| Motores Paso a Paso 28BYJ-48 | 3 | Actuadores espaciales (Ejes X, Y concurrentes, y Eje Z para el instrumento). Operan a 5V. |
| Drivers ULN2003 | 3 | Controladores de potencia (interfaz de aislamiento entre las señales del ESP32 y la energía de los motores) |
| Fuente de Alimentación ATX de PC | 1 | Provee energía constante de 5V con alto amperaje para el arranque simultáneo de motores |
| Breadboard / Cables Jumper (Dupont) | Varios | Conexiones entre el ESP32, Drivers y Fuente |
| Componentes impresos en 3D | Varios | Estructura mecánica, rieles y soporte de lápiz (Piezas A, B, C, D, E, F, G) |
| Instrumento de dibujo | 1 | Lápiz o birome |

---

## 🛠️ Usage Instructions
<!--
1. [Step 1: what to install or configure]
2. [Step 2: wiring / circuit setup]
3. [Step 3: how to upload and run the sketch]
4. [Step 4: expected output]

*(Include screenshots or execution examples if applicable)*
-->
1. **Configuración Inicial:** Conectar la fuente externa ATX a la protoboard y a los controladores ULN2003. Conectar el ESP32 a la computadora mediante cable USB, cargar el código desde el IDE de Arduino y reiniciar (desconectar/conectar).
2. **Conexión al Sistema:** Buscar en la PC o móvil la red WiFi `cnc-plotter` e ingresar la contraseña `12345678`. Abrir la interfaz HTML en el navegador y presionar el botón de conexión.
3. **Calibración:** En la pestaña "Calibración", configurar el Eje Z en 5 mm. Desde "Control", mover manualmente el cabezal a la esquina inferior izquierda. Ajustar el Eje Z hasta que el lápiz quede suspendido sobre el papel sin tocarlo. Definir este punto como "Cero (0,0,0)".
4. **Dibujo (Modos de operación):** 
   - *Modo Control:* Mover libremente usando las flechas web.
   - *Modo Imagen:* Cargar imágenes line-art, siluetas o logotipos (dimensionadas a máximo 60x60 mm).
   - *Modo Texto:* Escribir palabras a convertir en instrucciones de movimiento (Límite de papel 60 mm).

*(Precaución: Si el motor del Eje Z se desincroniza y empuja el lápiz contra la base sin avanzar, presionar "Detener" en la interfaz de inmediato para no quemar las bobinas).*

---

## 🧪 Simulation
<!-- - If possible, include a **Wokwi** simulation export of the project (diagram, sketch, and/or shareable link).
- Wokwi project link: [Enter link if available]
- Diagram/export files: [Enter file names/paths if included in the repo]
-->
- *Nota sobre la simulación:* Al tratarse de un ensamble mecánico complejo que coordina trayectorias físicas con piezas impresas en 3D, el proyecto está enfocado en su implementación física en hardware.
- Diagram/export files: Los diagramas de conexión, ensamblado y el código fuente se encuentran detallados en el informe del proyecto (`TPIntegrador_Grupo7`).

---

## 📝 Additional Notes
<!--
- [Challenges faced / technical decisions made]
- [Current limitations of the project]
- [Potential improvements for the future]
-->
- **Challenges faced / technical decisions made:**
  - *Pérdida de torque en Eje X:* Un falso contacto en un cable Dupont impedía la activación de una bobina, causando falta de fuerza. Se solucionó reemplazando el cable.
  - *Inversión del Eje Z:* El motor elevaba el lápiz al indicarle bajar. Se resolvió por software, invirtiendo las referencias lógicas de límite (dibujo=0.0mm, reposo=5.0mm).
  - *Prevención de colisiones:* Al no tener sensores, el cabezal chocaba contra los límites. Se creó la función predictiva `clampTravel()` en el ESP32 para limitar el recorrido máximo a 80mm.
- **Current limitations of the project:**
  - Sistema de Lazo Abierto: El ESP32 calcula los pasos pero carece de sensores de posición para confirmar que el movimiento se realizó. No puede compensar errores si la hoja se mueve o un motor pierde pasos.
- **Potential improvements for the future:**
  - Añadir un **joystick analógico** para control manual y dibujo a mano alzada.
  - Instalar un **botón de interrupción por hardware** para abortar el dibujo en emergencias de forma instantánea.
  - Implementar **pulsadores de final de carrera (Endstops)** en los ejes X e Y para detectar límites físicos y permitir autocalibración.
```eof
