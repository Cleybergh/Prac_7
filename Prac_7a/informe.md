# Práctica 7a

El objetivo de la practica actual es describir el funcionamiento del bus I2S y realizar una
practica para comprender su funcionamiento.

El protocolo I2S se usa para transferir señales de sonidos digitales.

Tiene 3 cables:
- El reloj serial (SCK): se usa para obtener todos los componentes en el mismo ciclo.
- Word Select (WS): diferenciael cana izquierdo del canal derecho, cuando esta en 0 és el canal izquierdo y el canal derecho es cuando está en 1.
- Serial Data (SD): 

## Código:
```
#include "Arduino.h"
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "SPI.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

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

## Descibir la salida por el puerto serie:

Cuando ejecutamos el programa podemos observar que se muestra por pantalla la prase "sound generator" cada vez que lo activamos.

## Funcionamiento

Primero añadimos las bibliotecas como siempre, en este caso para el amplificador "ESP8266Audio".

En el setup és donde importamos un audio, en "aac = new AudioGeneratorAAC();" asignamos a esta variable para poder descodificar el audio en el formato deseado. La variable out es para poder establecer la ganancia del audio "out = new AudioOutputI2S();".
Finalmente asignamos los pines de salida.

En el loop haces que el aac comience a funcionar hasta que para y se escribe por pantala "sound Generator" con un delay de 1 segundo.