# Practica 5b Escribir texto en una pantalla oled

### Código
````
#define __DEBUG__

#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

// Definir constantes
#define ANCHO_PANTALLA 128 // ancho pantalla OLED
#define ALTO_PANTALLA 64 // alto pantalla OLED

// Objeto de la clase Adafruit_SSD1306
Adafruit_SSD1306 display(ANCHO_PANTALLA, ALTO_PANTALLA, &Wire, -1);

void setup() {
#ifdef __DEBUG__
  Serial.begin(9600);
  delay(100);
  Serial.println("Iniciando pantalla OLED");
#endif

  // Iniciar pantalla OLED en la dirección 0x3C
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
#ifdef __DEBUG__
    Serial.println("No se encuentra la pantalla OLED");
#endif
    while (true);
  }

  // Limpiar buffer
  display.clearDisplay();

  // Tamaño del texto
  display.setTextSize(1);
  // Color del texto
  display.setTextColor(SSD1306_WHITE);
  // Posición del texto
  display.setCursor(10, 32);
  // Escribir texto
  display.println("Hola mundo!!");

  // Enviar a pantalla
  display.display();

}

void loop() {}
````

### Funcionamiento

En esta practica necesitaremos añadir librerías extra, en este caso serán las siguientes:
````
#include <SPI.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
````
Una vez añadidas las librerias, tendremos que definir dos constantes, las cuales seran el ancho de la pantallla y el alto de la pantalla. 

````
#define ANCHO_PANTALLA 128 
#define ALTO_PANTALLA 64 
````
Luego habra que crear un objeto de la clase Adafruit_SSD1306. El constructor de esta clase admite 4 parámetros, el ANCHO_PANTALLA, que es la constante donde almacenamos el ancho de la pantalla, el ALTO_PANTALLA, que es la constante donde almacenamos el alto de la pantalla, el &Wire, que es un puntero de la clase estática Wire, y finalmente el -1, que es el pin de ESP8266 que se utiliza para resetear la pantalla.

````
Adafruit_SSD1306 display(ANCHO_PANTALLA, ALTO_PANTALLA, &Wire, -1);
````
Seguidamente en el void setup() inicializamos una comunicación en serie a una velocidad de 9600 bauds, se hace un pequeño delay, y luego se hace una impresión por pantalla del mensaje: Iniciando pantalla OLED.

````
#ifdef __DEBUG__
  Serial.begin(9600);
  delay(100);
  Serial.println("Iniciando pantalla OLED");
#endif
````
Después iniciamos la pantalla OLED con la función display.begin(SSD1306_SWITCHCAPVCC, 0x3C), donde los parámetros son, SSD1306_SWITCHCAPVCC, el cual indica que se activa el voltage de 3,3V interno para la pantalla. Se puede utilizar una fuente externa utilizando la constante SSD1306_EXTERNALVCC, y el 0x3C, que es la dirección I2C que utiliza la pantalla.

En el caso de que la anterior funcion retorne un valor false nos querra decir que no se ha encontrado la pantalla OLED.
Y si se retorna true el código sigue.

````
if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
#ifdef __DEBUG__
    Serial.println("No se encuentra la pantalla OLED");
#endif
    while (true);
  }
````
A continuación con display.clearDisplay() limpiamos el buffer. Ahora las siguientes líneas de código son las encargadas de escribir un texto en la pantalla.

Lo primero de todo es establecer el tamaño del texto con:

display.setTextSize(1);
El tamaño 1 indica que las letras ocupan una altura de 8 píxeles de la pantalla.

El siguiente paso es establece el color:

display.setTextColor(SSD1306_WHITE);
Con SSD1306_WHITE el texto será de color blanco.

Ahora tenemos que situar el cursor en algún punto de la pantalla:

display.setCursor(10, 32);
Si hubiéramos puesto el cursor en (0,0) el texto comenzaría a pintarse a partir de las coordenadas (0,0) que se sitúan arriba y a la izquierda. Por lo tanto la coordenada (10,32) es un punto que esta más o menos en el medio de la pantalla.

Para escribir el texto en la pantalla OLED hay que utilizar la función:

display.println("Hola mundo!!");
Y ya para finalizar tenemos que enviar este texto a la pantalla con:

display.display();
El código del void setup() completo queda tal que así:
````
void setup() {
#ifdef __DEBUG__
  Serial.begin(9600);
  delay(100);
  Serial.println("Iniciando pantalla OLED");
#endif

  // Iniciar pantalla OLED en la dirección 0x3C
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
#ifdef __DEBUG__
    Serial.println("No se encuentra la pantalla OLED");
#endif
    while (true);
  }

  // Limpiar buffer
  display.clearDisplay();

  // Tamaño del texto
  display.setTextSize(1);
  // Color del texto
  display.setTextColor(SSD1306_WHITE);
  // Posición del texto
  display.setCursor(10, 32);
  // Escribir texto
  display.println("Hola mundo!!");

  // Enviar a pantalla
  display.display();

}
````