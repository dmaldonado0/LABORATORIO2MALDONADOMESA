# 🧪 LABORATORIO II – Sistemas Digitales

### INTEGRANTES:
- Daniel Alejandro Maldonado Silva
- David Esteban Mesa Gonzalez

## Objetivo General

Diseñar e implementar tres sistemas electrónicos que permitan evidenciar la integración entre software y hardware, utilizando Arduino como plataforma principal.

---

## Desarrollo del Proyecto

Para cumplir con el objetivo planteado, se desarrollaron los siguientes sistemas:

- **Chatbot interactivo**: permite la comunicación entre el usuario y Arduino mediante un programa en Python, utilizando comunicación serial para controlar LEDs y consultar la temperatura en tiempo real a través de un sensor.
  
    <img src="https://img.freepik.com/vector-gratis/chatbot-mensaje-chat-vectorart_78370-4104.jpg?semt=ais_hybrid&w=740&q=80" width="300"/>
- **Juego en pantalla OLED**: implementación de un sistema visual interactivo que hace uso de una pantalla OLED para representar elementos gráficos, permitiendo comprender el manejo de interfaces embebidas.
  
    <img src="https://www.coolmathgames.com/sites/default/files/styles/mobile_game_image/public/Snake_OG-logo.jpg.webp?itok=EOKYMydV" width="300"/>
- **Detector de colores con sensor CNY70**: sistema basado en la reflexión de luz infrarroja que permite identificar diferencias entre superficies, aplicando principios de sensado analógico.
  
  <img src="https://play-lh.googleusercontent.com/eIRQD_tBuVKgepklmPCD70LD2iP_HqRTNjSni6B3WDu9nnzErMjqvAiVdrXJuxU96sk" width="300"/>


---

## Alcance y Aplicación

A través de este laboratorio se refuerzan conceptos clave como:

- Comunicación serial entre dispositivos  
- Lectura e interpretación de sensores  
- Control de actuadores (LEDs)  
- Visualización de información en pantallas  
- Desarrollo de sistemas interactivos  

Estos conocimientos son fundamentales en el área de los sistemas digitales y tienen aplicaciones en automatización, robótica e Internet de las Cosas (IoT).

# Desarrollo del Proyecto

A continuación se describe el desarrollo de cada uno de los sistemas implementados en el laboratorio.

---

# 🔹 Punto 1: Chatbot + Control de LEDs + Monitoreo de Temperatura

##  Descripción
Sistema interactivo que permite controlar LEDs y consultar temperatura en tiempo real mediante un chatbot conectado a Arduino por comunicación serial.

## Objetivo
Implementar comunicación serial entre Python y Arduino para controlar actuadores y leer sensores.

## Materiales
- Arduino UNO  
- Sensor de temperatura  
- 2 LEDs  
- Resistencias  
- Protoboard y cables  
- Computador con Python  

## Conexión del Circuito
 <img src="" width="300"/>

## Funcionamiento
1. Usuario envía mensaje  
2. Python procesa el mensaje  
3. Se envía comando al Arduino  
4. Arduino ejecuta acción  
5. Se retorna respuesta  

## Código

### Arduino
```cpp
// Código Arduino aquí
```

### Python
```python
# Código chatbot aquí
```

## Resultados
- ✔ Control de LEDs funcional  
- ✔ Lectura de temperatura correcta  
- ✔ Comunicación estable  

# VIDEO FUNCIONAMIENTO:
https://unipanamericanaeduco-my.sharepoint.com/:f:/g/personal/danielamaldonado_ucompensar_edu_co/IgBtgWaWbDYTQbpJOH2Ys0HjAUxWp9R6gNYgSFfIyYJv83g?e=qnaglE
---

# 🔹 Punto 2: Juego en Pantalla OLED

## Descripción
Implementación de un juego básico utilizando una pantalla OLED controlada por Arduino.

## Objetivo
Desarrollar un sistema visual interactivo con salida gráfica.

## Materiales
- Arduino UNO  
- Pantalla OLED  

## Conexión del Circuito
 <img src="" width="300"/>

## Funcionamiento
1. Inicialización de pantalla  
2. Dibujado de gráficos  
3. Actualización continua  
4. Interacción o animación  

## Código
```cpp
// Código OLED aquí
```

## Resultados
- ✔ Visualización correcta  
- ✔ Funcionamiento fluido

# VIDEO FUNCIONAMIENTO:

---

# 🔹 Punto 3: Detector de Colores con CNY70

## Descripción
Sistema que detecta diferencias de color mediante reflexión usando el sensor CNY70.

## Objetivo
Interpretar señales analógicas provenientes de un sensor óptico.

## Materiales
- Arduino UNO  
- Sensor CNY70  
- Resistencias  
- Protoboard  

## Conexión del Circuito
 <img src="" width="300"/>

## Funcionamiento
1. Sensor emite luz  
2. Superficie refleja  
3. Se obtiene valor analógico  
4. Arduino interpreta  

## Código
```cpp
// Código sensor aquí
```

## Resultados
- ✔ Detección básica de colores  
- ✔ Lecturas estables  

# VIDEO FUNCIONAMIENTO:
https://unipanamericanaeduco-my.sharepoint.com/:f:/g/personal/danielamaldonado_ucompensar_edu_co/IgB1ZyB9bRz4Qbsz9a9YALgFAebuqkxCq2pCawgGBGJX8WQ?e=0L4Bs7

---

# Conclusión General

Se logró integrar hardware y software mediante Arduino y Python, desarrollando sistemas interactivos funcionales.

Se reforzaron conceptos como:
- Comunicación serial  
- Uso de sensores y actuadores  
- Visualización de datos  
- Sistemas digitales interactivos  

