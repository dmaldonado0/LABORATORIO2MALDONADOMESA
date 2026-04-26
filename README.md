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

### EXPLICACION:
En este sistema se desarrolló un chatbot que se comunica con un Arduino Uno mediante comunicación serial, permitiendo controlar LEDs y consultar datos de temperatura y humedad en tiempo real.

El funcionamiento se basa en la interacción entre un programa en Python y el Arduino. El usuario ingresa mensajes a través de la consola, los cuales son procesados por el programa en Python. Dependiendo del contenido del mensaje, se envían comandos específicos al Arduino a través del puerto serial.

El Arduino recibe estos comandos y ejecuta distintas acciones:

Cuando recibe el comando "msg", enciende ambos LEDs como indicación de actividad.
Cuando recibe "datos", el sensor DHT11 mide la temperatura y la humedad del ambiente, y envía estos valores de vuelta al programa en Python.
Si recibe "off", apaga los LEDs.

Por su parte, el programa en Python interpreta la respuesta del Arduino y muestra los datos al usuario de forma legible. Si el usuario pregunta por temperatura o humedad, el sistema responde con los valores obtenidos; de lo contrario, el chatbot indica que no reconoce la solicitud.

Durante las pruebas se comprobó que la comunicación serial es estable, permitiendo un intercambio de datos fluido entre ambos sistemas. Además, se logró una correcta lectura del sensor y un control efectivo de los LEDs, validando la integración entre software y hardware.

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

  for (int i = length - 1; i > 0; i--) {
    snakeX[i] = snakeX[i - 1];
    snakeY[i] = snakeY[i - 1];
  }

  if (dir == 0) snakeY[0]--;
  if (dir == 1) snakeX[0]++;
  if (dir == 2) snakeY[0]++;
  if (dir == 3) snakeX[0]--;

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

### EXPLICACION:
En este circuito se implementó un juego básico tipo “Snake” utilizando una pantalla OLED controlada por un Arduino Uno. El objetivo fue comprobar la capacidad del sistema para generar gráficos y permitir la interacción mediante botones.

La pantalla Pantalla OLED SSD1306 se encarga de mostrar los elementos del juego, mientras que los pulsadores permiten controlar el movimiento de la serpiente en distintas direcciones.

El funcionamiento del sistema se desarrolla de la siguiente manera:

Al iniciar, se configura la pantalla y se dibuja la serpiente en una posición inicial.
Mediante los botones, el usuario puede cambiar la dirección del movimiento (arriba, abajo, izquierda, derecha).
La serpiente se desplaza continuamente por la pantalla, actualizando su posición en cada ciclo del programa.
Se genera un “alimento” en una posición aleatoria; cuando la serpiente lo alcanza, aumenta su tamaño.
Si la serpiente sale de los límites de la pantalla, el juego se reinicia automáticamente.

Durante las pruebas se observó que el sistema responde correctamente a las entradas de los botones y actualiza los gráficos en tiempo real, logrando una animación fluida y una interacción efectiva entre el usuario y el sistema.

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

### EXPLICACION:

En este circuito se realizó una prueba del sensor óptico CNY70 con el fin de verificar su correcto funcionamiento y analizar los valores analógicos que proporciona.

El sensor emite luz infrarroja hacia una superficie, y dependiendo del color de esta, la cantidad de luz reflejada cambia. Esta variación se traduce en una señal analógica que el Arduino Uno puede interpretar.

Durante las pruebas se observó que:

Cuando el sensor detecta una superficie oscura (negra), la reflexión de luz es menor, lo que genera valores analógicos bajos (menores a 500). En este caso, el sistema enciende el LED.
Cuando detecta una superficie clara (blanca), la reflexión es mayor, produciendo valores altos (mayores a 500), lo que provoca que el LED se apague.

De esta manera, el circuito permite diferenciar entre superficies claras y oscuras utilizando un umbral definido, validando el correcto funcionamiento tanto del sensor como del sistema de lectura analógica.

---

# Conclusión General

Se logró integrar hardware y software mediante Arduino y Python, desarrollando sistemas interactivos funcionales.

Se reforzaron conceptos como:
- Comunicación serial  
- Uso de sensores y actuadores  
- Visualización de datos  
- Sistemas digitales interactivos  

