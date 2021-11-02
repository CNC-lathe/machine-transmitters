#include <SoftwareSerial.h>

SoftwareSerial BTport(10,11); //D10 is RX, D11 is TX

//const int spndlPin = 2;
const int doorPin = 4;

uint8_t doorState;         //Current state of door (open/shut;low/high) - field 1
uint16_t spndlSpd;         //Spindle speed - field 2
uint16_t chkSum;           //Checksum = spindle speed + door state - field 3
uint8_t fldDlim = 44;      //Field Delimiter - ASCII standard for ","
uint8_t msgDlim = 59;      //Message Delimiter - ASCII standard for ";"
uint8_t TXdata[8];         //Transmission Data allocated to a single array

void setup() {
  BTport.begin(9600);       //Set data rate for software serial port
  //pinMode(spndlPin, INPUT); //Designate spindle speed pin 2 as an input
  pinMode(doorPin, INPUT);  //Designate door status pin 4 as an input
}

void loop() {
  
  if (digitalRead(doorPin) == HIGH){doorState = 1;}
  else {doorState = 0;}
  
  spndlSpd = 4999;
  chkSum = spndlSpd + doorState;
  
  TXdata[0] = doorState;
  TXdata[1] = fldDlim;
  TXdata[2] = highByte(spndlSpd);
  TXdata[3] = lowByte(spndlSpd);
  TXdata[4] = fldDlim;
  TXdata[5] = highByte(chkSum);
  TXdata[6] = lowByte(chkSum);
  TXdata[7] = msgDlim;
  
  if(BTport.isListening()){         //Verify host is available to receive data
    BTport.write(TXdata, 8);        //Send data as raw binary to host
  }
  delay(1600);                      //Wait 1.6 ms before repeating transmission
}
//READ WITH REALTERM SERIAL SOFTWARD FOR TESTING OUTPUT DATA STREAM
