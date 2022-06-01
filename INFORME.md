Materiales:
· Amplificador de Audio Módulo decodificador I2S Dac sin filtrar
· Altavoz 
· ESP32

Presentación:
En esta pràctica vamos a observar los buses de I2S. En la red I2S siempre tenemos a alguien que transmite y 
a alguien que recibe. Ambos puede ser el maestro(no a la vez), y no solo eso sinó que podemos añadir un 
controlador externo. Todo aquel que genere la señal de reloj (SCK) y la señal de las líneas de selección de palabras(WS),
serà considerado maestro.
El amplificador sirve como un decodificador que de señales I2S a señales analogicas, que es lo que entiende el altavoz.
Lo hace gracias a un conversor digital analógico(DAC) y tambien con un amplificador incorporado que augmenta la señal de la señal analógica.
El amplificador conecta con el altavoz usando dos cables conectados en +(rojo) y -(negro).
El amplificador conecta con la placa ESP32 usando:
LRC ->G25
BCK->G26
DIN->G22
GND->GND
VIN-> 3.3V


Explicación del código:
```
#include <Arduino.h>
//librerias 
 #include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
//creamos un objeto puntero del tipo de las librerias añadidas
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup(){
  Serial.begin(115200);

  in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
  aac = new AudioGeneratorAAC();
  out = new AudioOutputI2S();
  out -> SetGain(0.125);
  out -> SetPinout(26,25,22);
  aac->begin(in, out);
}

void loop(){
  if (aac->isRunning()) {
    aac->loop();
  } else {
    aac -> stop();
    Serial.printf("Sound Generator\n");
    delay(1000);
  }
}

```

Ver salida en el vídeo
