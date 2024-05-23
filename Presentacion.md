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
#### Ingresamos a https://processing.org/download
![Captura de pantalla](https://raw.githubusercontent.com/AbnerOrterga98/Repo-Final-2PM/main/procesing.png)
#### y damos click a instalar la version mas nueva
#### Comprimos el archivo y tardara unos minutos en instalarse
---
#### Una ves ya instalado se hizo click en Procesing
#### Aparecera la siguiente carpeta la abrimos y damos click en Procesing
![Captura de pantalla](https://raw.githubusercontent.com/AbnerOrterga98/Repo-Final-2PM/main/Captura%20de%20pantalla%20.png)
#### Aparecera una consola donde se realizara el codigo para leer un flujo de datos en formato RGB565 
#### desde el puerto serial y mostrar una imagen en una ventana.
### El codigo que se hizo fue el siguiente
/*
  This sketch reads a raw Stream of RGB565 pixels
  from the Serial port and displays the frame on
  the window.

  Use with the Examples -> CameraCaptureRawBytes Arduino sketch.

  This example code is in the public domain.
*/

import processing.serial.*;
import java.nio.ByteBuffer;
import java.nio.ByteOrder;

Serial myPort;

// must match resolution used in the sketch
final int cameraWidth = 320;
final int cameraHeight = 240;
final int cameraBytesPerPixel = 2;
final int bytesPerFrame = cameraWidth * cameraHeight * cameraBytesPerPixel;

PImage myImage;
byte[] frameBuffer = new byte[bytesPerFrame];

void setup()
{
  size(320, 240);

  // if you have only ONE serial port active
  //myPort = new Serial(this, Serial.list()[0], 9600);          // if you have only ONE serial port active

  // if you know the serial port name
  myPort = new Serial(this, "COM13", 9600);                    // Windows
  //myPort = new Serial(this, "/dev/ttyACM0", 9600);            // Linux
  //myPort = new Serial(this, "/dev/cu.usbmodem14401", 9600);     // Mac

  // wait for full frame of bytes
  myPort.buffer(bytesPerFrame);  

  myImage = createImage(cameraWidth, cameraHeight, RGB);
}

void draw()
{
  image(myImage, 0, 0);
}

void serialEvent(Serial myPort) {
  // read the saw bytes in
  myPort.readBytes(frameBuffer);

  // access raw bytes via byte buffer
  ByteBuffer bb = ByteBuffer.wrap(frameBuffer);
  bb.order(ByteOrder.BIG_ENDIAN);

  int i = 0;

  while (bb.hasRemaining()) {
    // read 16-bit pixel
    short p = bb.getShort();

    // convert RGB565 to RGB 24-bit
    int r = ((p >> 11) & 0x1f) << 3;
    int g = ((p >> 5) & 0x3f) << 2;
    int b = ((p >> 0) & 0x1f) << 3;

    // set pixel color
    myImage .pixels[i++] = color(r, g, b);
  }
 myImage .updatePixels();

}
#### Nos dirigimos a Arduino IDE y damos click en Examples > Arduino_OV767X > CameraCaptureRawBytes
![Captura de pantalla](https://raw.githubusercontent.com/AbnerOrterga98/Repo-Final-2PM/main/Captura%20de%20pantalla%20.png)
#### y nos aparecera el siguiente codigo
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

#### damos click en Upload 
#### Una vez cargado nos dirigimos a procesing y tambien lo compilamo

---
## Problemas Encontrados
![Captura de pantalla](https://raw.githubusercontent.com/AbnerOrterga98/Repo-Final-2PM/main/Captura%20de%20pantalla%20(231).png)



 

