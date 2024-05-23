# Presentacion Proyecto 5: Captura de Foto via Csharp o Python con ATML kit

## Alumno:

### Ortega Medina Abner Nahum

## Materia

### Lenguaje de interfaz

## Docente

### Rene Solis Reyes

## Fecha

### 22 de mayo de 2024

---

## Proceso de desarrollo

### Lo primero que se hizo cumplir con todos los requirimientos previamente pedidos como:
#### Arduino Tiny Machine Learning Kit.
#### Cámara compatible con Arduino (módulo de cámara).
#### Botón o sensor para activar la cámara.
#### Cables de conexión.
#### Computadora con el software de Arduino instalado.
#### GitRepo de kit con ejemplos

#### Para despues ingresar a Arduino IDE para instalar las siguientes bibliotecas
#### Pero antes conectamos el arduino con un cable MicroUsb
#### Nos dirigimos a Tools > Manage libraries y buscar la bibliotecas correspondientes:
#### Arduino Mbed Os Nano Boards
#### Damos click en instalar
#### Aparece la opcion Arduino Nano 33 BLE que corresponde al arduino que se utilizara
#### En este punto hay que encontrar el puerto correspondiente en este caso es COM13
#### Volvemos a ingresar a dar click en Tools > Manage libraries y buscar la biblioteca OV767
#### Nos aparecera la biblioteca Arduino_OV767
#### Damos click en instalar
#### Tambien se descargaron las bibliotecas Harvard_TinyMLx,TensorFlowLite_ESP32.
#### Una vez este paso vamos a instalar 
#### Ahora damos click en Examples > Arduino_OV767X > CameraCaptureRawBytes
#### Ingresamos a https://processing.org/download
#### y damos click a instalar la version mas nueva
#### Comprimos el archivo y tardara unos minutos en instalarse
#### copiamos el codigo de CameraCaptureRawBytes
*
  OV767X - Camera Capture Raw Bytes

  This sketch reads a frame from the OmniVision OV7670 camera
  and writes the bytes to the Serial port. Use the Procesing
  sketch in the extras folder to visualize the camera output.

  Circuit:
    - Arduino Nano 33 BLE board
    - OV7670 camera module:
      - 3.3 connected to 3.3
      - GND connected GND
      - SIOC connected to A5
      - SIOD connected to A4
      - VSYNC connected to 8
      - HREF connected to A1
      - PCLK connected to A0
      - XCLK connected to 9
      - D7 connected to 4
      - D6 connected to 6
      - D5 connected to 5
      - D4 connected to 3
      - D3 connected to 2
      - D2 connected to 0 / RX
      - D1 connected to 1 / TX
      - D0 connected to 10

  This example code is in the public domain.
*/

#include <Arduino_OV767X.h>

int bytesPerFrame;

byte data[320 * 240 * 2]; // QVGA: 320x240 X 2 bytes per pixel (RGB565)

void setup() {
  Serial.begin(9600);
  while (!Serial);

  if (!Camera.begin(QVGA, RGB565, 1)) {
    Serial.println("Failed to initialize camera!");
    while (1);
  }

  bytesPerFrame = Camera.width() * Camera.height() * Camera.bytesPerPixel();

   //Optionally, enable the test pattern for testing
   //Camera.testPattern();
}

void loop() {
  Camera.readFrame(data);

  Serial.write(data, bytesPerFrame);
}

---
## Problemas Encontrados

#### Se presentaron algunos problemas





 

