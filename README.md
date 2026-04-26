 <img src="https://www.elempleo.com/co/sitio-empresarial/CompanySites/unicompensar/resources/images/logo-social.png" width="500"/>

# 🧪 LABORATORIO II – Sistemas Digitales

### INTEGRANTES:
- Daniel Alejandro Maldonado Silva
  danielamaldonado@ucompensar.edu.co
- David Esteban Mesa Gonzalez
  destebanmesa@ucompensar.edu.co

ASIGNATURA:

- *SISTEMAS DIGITALES*

DOCENTE:
- *Diego Alejandro Barragan Vargas*


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
 <img src="https://github.com/dmaldonado0/LABORATORIO2MALDONADOMESA/blob/main/TEMPPP.jpeg" width="300"/>

## Funcionamiento
1. Usuario envía mensaje  
2. Python procesa el mensaje  
3. Se envía comando al Arduino  
4. Arduino ejecuta acción  
5. Se retorna respuesta  

## Código

### Arduino
```cpp
#include <DHT.h>

#define DHTPIN 7
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

#define LED_ROJO 8
#define LED_VERDE 9

void setup() {
  Serial.begin(9600);

  pinMode(LED_ROJO, OUTPUT);
  pinMode(LED_VERDE, OUTPUT);

  dht.begin();
}

void loop() {

  if (Serial.available()) {
    String comando = Serial.readStringUntil('\n');
    comando.trim();

    if (comando == "msg") {
      digitalWrite(LED_ROJO, HIGH);
      digitalWrite(LED_VERDE, HIGH);
    }

    else if (comando == "datos") {
      float t = dht.readTemperature();
      float h = dht.readHumidity();

      if (isnan(t) || isnan(h)) {
        Serial.println("ERROR");
      } else {
        Serial.print(t);
        Serial.print(",");
        Serial.println(h);
      }
    }

    else if (comando == "off") {
      digitalWrite(LED_ROJO, LOW);
      digitalWrite(LED_VERDE, LOW);
    }
  }
}
```

### Python
```python
import serial
import time

arduino = serial.Serial('COM3', 9600) 
time.sleep(2)

print("Chatbot listo")

while True:
    msg = input("Tú: ").lower()

   
    arduino.write(b"msg\n")

    if "temperatura" in msg or "humedad" in msg:
        arduino.write(b"datos\n")
        time.sleep(1)

        if arduino.in_waiting:
            data = arduino.readline().decode().strip()

            if data == "ERROR":
                print("Bot: Error leyendo sensor")
            else:
                temp, hum = data.split(",")

                print(f"Manito La Temperatura es: {temp} °C")
                print(f"UY Mano La Humedad: {hum} %")

    else:
        print("Bot: lirul hand no se de eso, Pregúntame por la temperatura o la humedad")

    arduino.write(b"off\n")
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
- Pulsadores 

## Conexión del Circuito
 <img src="https://github.com/dmaldonado0/LABORATORIO2MALDONADOMESA/blob/main/imagen%20juego.jpeg" width="300"/>

## Funcionamiento
1. Inicialización de pantalla  
2. Dibujado de gráficos  
3. Actualización continua  
4. Interacción o animación  

## Código
```cpp
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <Wire.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

#define BTN_UP 2
#define BTN_DOWN 3
#define BTN_LEFT 4
#define BTN_RIGHT 5

#define SIZE 4

int snakeX[100];
int snakeY[100];
int length = 5;

// Dirección (0=arriba,1=derecha,2=abajo,3=izquierda)
int dir = 1;

int foodX, foodY;

unsigned long lastPress = 0;
int debounceDelay = 120;

void spawnFood() {
  foodX = random(0, SCREEN_WIDTH / SIZE);
  foodY = random(0, SCREEN_HEIGHT / SIZE);
}

void setup() {
  pinMode(BTN_UP, INPUT_PULLUP);
  pinMode(BTN_DOWN, INPUT_PULLUP);
  pinMode(BTN_LEFT, INPUT_PULLUP);
  pinMode(BTN_RIGHT, INPUT_PULLUP);

  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();

  // Inicializar snake
  for (int i = 0; i < length; i++) {
    snakeX[i] = 10 - i;
    snakeY[i] = 10;
  }

  spawnFood();
}

void loop() {

  if (millis() - lastPress > debounceDelay) {

    if (digitalRead(BTN_UP) == LOW && dir != 2) {
      dir = 0;
      lastPress = millis();
    }

    else if (digitalRead(BTN_RIGHT) == LOW && dir != 3) {
      dir = 1;
      lastPress = millis();
    }

    else if (digitalRead(BTN_DOWN) == LOW && dir != 0) {
      dir = 2;
      lastPress = millis();
    }

    else if (digitalRead(BTN_LEFT) == LOW && dir != 1) {
      dir = 3;
      lastPress = millis();
    }
  }

  // Mover snake
  for (int i = length - 1; i > 0; i--) {
    snakeX[i] = snakeX[i - 1];
    snakeY[i] = snakeY[i - 1];
  }

  if (dir == 0) snakeY[0]--;
  if (dir == 1) snakeX[0]++;
  if (dir == 2) snakeY[0]++;
  if (dir == 3) snakeX[0]--;

  // Comer comida
  if (snakeX[0] == foodX && snakeY[0] == foodY) {
    length++;
    spawnFood();
  }

  if (snakeX[0] < 0 || snakeX[0] >= SCREEN_WIDTH / SIZE ||
      snakeY[0] < 0 || snakeY[0] >= SCREEN_HEIGHT / SIZE) {

    length = 5;
    dir = 1;

    for (int i = 0; i < length; i++) {
      snakeX[i] = 10 - i;
      snakeY[i] = 10;
    }
  }

  display.clearDisplay();

  for (int i = 0; i < length; i++) {
    display.fillRect(snakeX[i] * SIZE, snakeY[i] * SIZE, SIZE, SIZE, WHITE);
  }

  display.fillRect(foodX * SIZE, foodY * SIZE, SIZE, SIZE, WHITE);

  display.display();

  delay(100);
}
```

## Resultados
- ✔ Visualización correcta  
- ✔ Funcionamiento fluido

# VIDEO FUNCIONAMIENTO:
https://unipanamericanaeduco-my.sharepoint.com/:f:/g/personal/danielamaldonado_ucompensar_edu_co/IgCBzYAvCB9wTacga_oiqWPwAeaBEQxE5fAlWVDAlyHrvok?e=ie5zZf
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
 <img src="https://github.com/dmaldonado0/LABORATORIO2MALDONADOMESA/blob/main/imagencolormejor.jpeg" width="300"/>

## Funcionamiento
1. Sensor emite luz  
2. Superficie refleja  
3. Se obtiene valor analógico  
4. Arduino interpreta  

## Código
```cpp
const int sensorPin = A0;  
const int ledPin = 3;      

int umbral = 500;          

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int valor = analogRead(sensorPin);
  Serial.println(valor);

  if (valor < umbral) {
    digitalWrite(ledPin, HIGH); // Negro LED encendido
  } else {
    digitalWrite(ledPin, LOW);  // Blanco LED apagado
  }

  delay(100);
}
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

