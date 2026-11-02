# 📡 Comunicación digital inalámbrica GFSK con Raspberry Pi Pico 2W y NRF24L01

Implementación y análisis de un enlace inalámbrico en la banda ISM de 2,4 GHz, utilizando dos tarjetas Raspberry Pi Pico 2W y módulos transceptores nRF24L01. El proyecto incluye análisis de la interfaz SPI, pruebas de alcance, evaluación de errores de transmisión y caracterización espectral.

## 🎯 Objetivos

- Implementar y analizar la comunicación SPI entre la Raspberry Pi Pico 2W y el nRF24L01.
- Estimar el alcance teórico y contrastarlo con pruebas de campo.
- Evaluar la tasa de error de bits (BER) mediante archivos de alfabetos y datos pseudoaleatorios.
- Verificar la frecuencia de operación y estimar el ancho de banda de la señal.
- Examinar la distribución de canales y el posible traslape con señales Wi-Fi en 2,4 GHz.

## 🧰 Materiales y herramientas

- 2 Raspberry Pi Pico 2W
- 2 módulos nRF24L01 con antena
- Pantallas OLED I2C
- Pulsadores y LED indicadores
- Analizador lógico
- Analizador de espectros Anritsu Spectrum Master MS2724B
- Cables de conexión y protoboard

## 🔌 Conexiones del sistema

El transmisor y el receptor utilizan la misma distribución de conexiones entre la Raspberry Pi Pico 2W y el nRF24L01.

### nRF24L01 ↔ Raspberry Pi Pico 2W

| Pin del nRF24L01 | GPIO de la Pico 2W | Pin físico | Función |
|---|---:|---:|---|
| VCC | 3V3(OUT) | 36 | Alimentación |
| GND | GND | 38 | Tierra |
| CE | GP20 | 26 | Control del transceptor |
| CSN | GP17 | 22 | Selección del dispositivo SPI |
| SCK | GP18 | 24 | Reloj SPI |
| MOSI | GP19 | 25 | Datos de salida |
| MISO | GP16 | 21 | Datos de entrada |
| IRQ | Sin conectar | — | No utilizada |

> ⚠️ El módulo nRF24L01 se alimenta desde la salida de **3,3 V** de la Pico 2W.

### Pantalla OLED I2C ↔ Raspberry Pi Pico 2W

| Pin de la OLED | GPIO de la Pico 2W | Pin físico |
|---|---:|---:|
| VCC | 3V3(OUT) | 36 |
| GND | GND | 38 |
| SDA | GP4 | 6 |
| SCL | GP5 | 7 |

### Pulsadores

| Función | GPIO | Pin físico | Uso |
|---|---:|---:|---|
| Selección de canal | GP10 | 14 | Transmisor y receptor |
| Selección de potencia | GP11 | 15 | Transmisor y receptor |
| Envío de mensaje | GP12 | 16 | Solo transmisor |

### LED indicadores

| Indicador | GPIO | Pin físico | Conexión |
|---|---:|---:|---|
| OK | GP21 | 27 | LED con resistencia de 220–330 Ω hacia GND |
| Error | GP22 | 29 | LED con resistencia de 220–330 Ω hacia GND |

El pin físico 28, correspondiente a GND, se ubicó entre las conexiones de los dos LED.

## 🔍 Conexión del analizador lógico

El analizador lógico se conectó al transmisor (Pico 1) para observar las señales de la interfaz SPI.

| Canal del analizador | Canal en Logic 2 | Señal | GPIO de la Pico | Pin físico |
|---|---|---|---|---:|
| CH1 | Canal 0 | CSN | GP17 | 22 |
| CH2 | Canal 1 | SCK | GP18 | 24 |
| CH3 | Canal 2 | MOSI | GP19 | 25 |
| CH4 | Canal 3 | MISO | GP16 | 21 |
| CH5 | Canal 4 | CE | GP20 | 26 |
| GND | — | Tierra común | GND | 23 |

## ⚙️ Configuración de comunicación

| Parámetro | Configuración |
|---|---|
| Banda de operación | 2,4 GHz ISM |
| Canal utilizado en pruebas de alcance y BER | 76 |
| Frecuencia del canal 76 | 2476 MHz |
| Velocidades de transmisión | 250 kbps, 1 Mbps y 2 Mbps |
| Potencias de transmisión | −18, −12, −6 y 0 dBm |
| Interfaz con el transceptor | SPI |
| Interfaz de pantalla | I2C |

## 🧪 Procedimiento experimental

### 1. Análisis de la interfaz SPI

Se utilizó el analizador lógico para capturar operaciones de lectura y escritura de registros del nRF24L01, así como la transferencia de paquetes. En las capturas se identificaron los comandos, bytes transmitidos, señales de control y tiempos de comunicación.

### 2. Análisis de alcance

Se calcularon alcances teóricos mediante las pérdidas de propagación en espacio libre, considerando las potencias de transmisión y sensibilidades del receptor. Posteriormente, se realizaron pruebas de campo con diferentes velocidades, potencias y distancias.

### 3. Análisis de errores

Se transmitieron archivos de texto con alfabetos y datos pseudoaleatorios. La información recibida se comparó con el archivo original para contabilizar los bits erróneos y calcular la BER.

### 4. Análisis espectral

Con el analizador de espectros se verificó la frecuencia de operación, se estimó el ancho de banda de la señal y se examinó la ocupación de la banda de 2,4 GHz, incluyendo la actividad de canales Wi-Fi.

## 📊 Resultados principales

- La captura SPI permitió identificar operaciones de lectura, escritura y carga de paquetes, además de comprobar que transmisor y receptor deben operar en el mismo canal.
- El alcance teórico calculado estuvo entre **15,3 m y 483,7 m**, según la potencia y la velocidad configuradas.
- En las pruebas de campo se observaron diferencias frente al modelo ideal, asociadas a las condiciones del entorno y a la actividad de otros sistemas en 2,4 GHz.
- La BER medida estuvo entre **0 y 3,7 × 10⁻³**. Los resultados mostraron errores concentrados en algunos paquetes y no una tendencia uniforme con la potencia.
- El analizador de espectros confirmó la portadora en **2,476 GHz**, correspondiente al canal 76. El ancho de banda estimado fue de aproximadamente **1,5 MHz**, medido con los marcadores del equipo.

## 🛠️ Tecnologías y equipos

- MicroPython
- Raspberry Pi Pico 2W
- Transceptor nRF24L01
- Comunicación SPI e I2C
- Logic 2
- Analizador lógico
- Anritsu Spectrum Master MS2724B

## 📄 Informe

El informe completo, con el procedimiento experimental, capturas, tablas de resultados y conclusiones, se encuentra en el repositorio

---

**Universidad Militar Nueva Granada**  

- **Asignatura:** Comunicaciones Digitales
- **Programa:** Ingeniería en Telecomunicaciones
- **Universidad:** Universidad Militar Nueva Granada
- **Periodo:** 2026-2
- **Docente:** Ing. José de Jesús Rugeles Uribe

## 👥 Autores

- **Harol Felipe Riveros Sierra** — 1401660
- **Salome Bohorquez Blanco** — 1401654
