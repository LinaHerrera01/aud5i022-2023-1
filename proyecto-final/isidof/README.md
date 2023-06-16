PROYECTO FINAL


    Estoy trabajando con [LinaHerrera01](https://github.com/linaherrera01)

    Herramientas disponibles, 2 kits de Arduino UNO, potenciómetro flex sensor, micrófono acústico (conector de entrada Jack 6.3mm Mono)...

Posibles usos del potenciómetro lineal flexible: Robótica, Videojuegos (movimiento virtual), dispositivos médicos, componentes externos de PC, instrumentos musicales, terapia física ([DigiKey](https://www.digikey.com/en/product-highlight/s/spectra-symbol/flex-sensor))

EXPERIENCIA MUSICAL INTERACTIVA


¿Es posible participar e influir en una proyección de sonido a tiempo real mediante un flex sensor?

Efectos de sonido disponibles en [Processing](https://processing.org/reference/libraries/sound/index.html):

[BandPass](https://processing.org/reference/libraries/sound/BandPass.html)

[Delay](https://processing.org/reference/libraries/sound/Delay.html)

[HighPass](https://processing.org/reference/libraries/sound/HighPass.html)

[LowPass](https://processing.org/reference/libraries/sound/LowPass.html)

[Reverb](https://processing.org/reference/libraries/sound/Reverb.html)

* 
const int flexPin = A0; // Pin analógico utilizado para el sensor flex
int flexValue; // Valor leído del sensor flex

void setup() {
  Serial.begin(9600); // Iniciar comunicación serie a 9600 baudios
}

void loop() {
  flexValue = analogRead(flexPin); // Leer el valor del sensor flex
  Serial.println(flexValue); // Enviar el valor por la comunicación serie
  delay(100); // Esperar un breve período antes de leer nuevamente
}
*


*
import processing.serial.*;
import processing.sound.*;

Serial arduino; // Objeto Serial para la comunicación con Arduino
AudioIn input; // Objeto de entrada de audio
Delay delay; // Objeto de efecto de retardo
Reverb reverb; // Objeto de efecto de reverberación

void setup() {
  size(400, 200);
  arduino = new Serial(this, Serial.list()[0], 9600); // Inicializar la comunicación con Arduino
  
  // Configuración de efectos de audio
  input = new AudioIn(this, 0);
  delay = new Delay(this);
  reverb = new Reverb(this);
  
  input.start();
  delay.process(input);
  reverb.process(input);
  
  // Establecer valores iniciales de los efectos
  delay.delayTime(0.5);
  reverb.amount(0.5);
}

void draw() {
  if (arduino.available() > 0) {
    String data = arduino.readStringUntil('\n'); // Leer datos de Arduino hasta el salto de línea
    if (data != null) {
      data = data.trim(); // Eliminar espacios en blanco al inicio y al final
      int flexValue = Integer.parseInt(data); // Convertir el valor a entero
      println("Flex value: " + flexValue);
      
      // Mapear el valor del sensor flex al rango de efectos de audio
      float effectValue = map(flexValue, 0, 1023, 0, 1); // Ajustar según tus necesidades
      
      // Aplicar los efectos de audio en función del valor del sensor flex
      audioEffect(effectValue);
    }
  }
}

void audioEffect(float value) {
  // Cambiar el efecto de retardo en función del valor del sensor flex
  float delayTime = map(value, 0, 1, 0.1, 1); // Mapear el valor del sensor flex al rango de tiempo de retardo
  delay.delayTime(delayTime);
  
  // Cambiar el efecto de reverberación en función del valor del sensor flex
  float reverbAmount = map(value, 0, 1, 0, 1); // Mapear el valor del sensor flex al rango de cantidad de reverberación
  reverb.amount(reverbAmount);
}

*



Links con referencias 


[ITP_NYC](https://vimeo.com/itpred)

[SumoZade](https://www.sumozade.com/es/product/el-potenciometro-lineal-flexible)

[ProcessingSound](https://processing.org/reference/libraries/sound/index.html)

