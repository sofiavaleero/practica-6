## Practica 6 Parte 1
***Raul Gonzalez / Sofia Valero***
## Codigo:

```
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN	21    //Pin 9 para el reset del RC522
#define SS_PIN	5   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

void setup() {
	Serial.begin(115200); //Iniciamos la comunicación  serial
	SPI.begin();        //Iniciamos el Bus SPI
	mfrc522.PCD_Init(); // Iniciamos  el MFRC522
	Serial.println("Lectura del UID");
}

void loop() {
	// Revisamos si hay nuevas tarjetas  presentes
	if ( mfrc522.PICC_IsNewCardPresent()) 
        {  
  		//Seleccionamos una tarjeta
            if ( mfrc522.PICC_ReadCardSerial()) 
            {
                  // Enviamos serialemente su UID
                  Serial.print("Card UID:");
                  for (byte i = 0; i < mfrc522.uid.size; i++) {
                          Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
                          Serial.print(mfrc522.uid.uidByte[i], HEX);   
                  } 
                  Serial.println();
                  // Terminamos la lectura de la tarjeta  actual
                  mfrc522.PICC_HaltA();         
            }      
	}	
}

```
### Salida:

Subimos el programa a la placa. Este hace que al pasar una tarjeta/llavero por delante del sensor que estamos utilizando, se vea por la pantalla del ordenador el código de la tarjeta o llavero que estemos usando en ese momento, como podemos ver en la siguiente imagen:

![](lecturauid.jpg)

### Salida:
Para poder usar el sensor primeramente hemos de instalar las librerias necessarias y declararlas, seguido de la definicion de los pines que vamos a necessitar y el reset:

```
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN	21    //Pin 9 para el reset del RC522
#define SS_PIN	5   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

```
Dentro del setup inicializamos la comunicación serial, una vez hecho estro inicializamos el SPI y el MFRC522. Si todo va bien saldra por pantalla un mensaje que nos dira que ya podemos pasar la tarjeta o e llavero por el sensor.

```


void setup() {
	Serial.begin(115200); //Iniciamos la comunicación  serial
	SPI.begin();        //Iniciamos el Bus SPI
	mfrc522.PCD_Init(); // Iniciamos  el MFRC522
	Serial.println("Lectura del UID");
}
```
Finalmente en el loop trabajamos con el sensor. Primeramente el sensor va ir mirando si algo pasa por delante suyo, a continuacion escoje la tarjeta/llavero que le hemos pasado por denalte y nos muestra por pantalla su UID y finaliza la lectura.

```
void loop() {
	// Revisamos si hay nuevas tarjetas  presentes
	if ( mfrc522.PICC_IsNewCardPresent()) 
        {  
  		//Seleccionamos una tarjeta
            if ( mfrc522.PICC_ReadCardSerial()) 
            {
                  // Enviamos serialemente su UID
                  Serial.print("Card UID:");
                  for (byte i = 0; i < mfrc522.uid.size; i++) {
                          Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
                          Serial.print(mfrc522.uid.uidByte[i], HEX);   
                  } 
                  Serial.println();
                  // Terminamos la lectura de la tarjeta  actual
                  mfrc522.PICC_HaltA();         
            }      
	}	
}
```