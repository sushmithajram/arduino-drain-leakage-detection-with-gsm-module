/* author             : j.sushmitha
 * thanks and credits : teachers and seniors of iota batch3
DRAIN WATER LEAKAGE DETECTION 
alert after detecting water leakage in drain pipe
connect usb cabe of arduino to pc
2nd digital pin of uno to water flow sensor
 */

#include<SoftwareSerial.h>
SoftwareSerial mySerial(9,10);
const int buzzer     = 10;
byte statusLed       = 13;
byte sensorInterrupt = 0; 
byte sensorPin       = 2;                                                    
volatile byte pulseCount;  
float flowRate;
unsigned long oldTime;

void setup()
{
  Serial.begin(9600);                                                                 
  pinMode(buzzer,OUTPUT);                                                             
  pinMode(statusLed, OUTPUT);                                                        
                                                   
  pinMode(sensorPin, INPUT);
  digitalWrite(sensorPin, HIGH);                                                     

  pulseCount        = 0;
  flowRate          = 0.0;
  oldTime           = 0;
  
 attachInterrupt(sensorInterrupt, pulseCounter, FALLING);                           
}

void loop()
{
   
   if((millis() - oldTime) > 1000)                                                     
  { 
    detachInterrupt(sensorInterrupt);                                                  
    flowRate = ((1000.0 / (millis() - oldTime)) * pulseCount) / 4.5;     
    oldTime = millis();                                                                
                                                 
      
    unsigned int frac;
   
    Serial.print("Flow rate: ");                                                       
    Serial.print(int(flowRate));                                                       
    Serial.print("L/min");
    Serial.print("\t");      
 while(flowRate> 10){                            
    Serial.println("**************** \t\t LEAK \t\t ****************");                
    delay(1000);
    digitalWrite(statusLed, HIGH); 
    tone(buzzer,1000);
    delay(1000);
	
	
	/*----> CALLING <-----*/
{Serial.println("Calling through GSM Modem");
mySerial.begin(9600);
delay(2000);
mySerial.println("ATD9444995601;");
Serial.println("Called ATD9444995601");
delay(30000);
if(mySerial.available())
Serial.write(mySerial.read());}
    
   } 
    pulseCount = 0;                                                                 
   attachInterrupt(sensorInterrupt, pulseCounter, FALLING);                         
  }
}

/*
Interrupt Service Routine
 */
void pulseCounter()
{
  pulseCount++;                                                                      
}
