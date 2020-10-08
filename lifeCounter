#include <LedControl.h>
#include <Entropy.h>

LedControl lc = LedControl(12,11,10,1);

const int rollSwitch = 2; // momentary switch/button for dice roll
const int resetSwitch = 5; // momentary switch/button for reset to full life
const int upA = 4; // momentary switch/button player A +1
const int downA = 3; // momentary switch/button player A -1 life
const int upB = 6; // momentary switch/button player B +1 life
const int downB = 7; // momentary switch/button player B -1 life
const int potPin = A0; // potentiometer to set display brightness

uint8_t rollSwitchState = 0;
uint8_t resetSwitchState = 0;
uint8_t upAState = 0;
uint8_t downAState = 0;
uint8_t upBState = 0;
uint8_t downBState = 0;
uint16_t  potVal = 0;

uint8_t digit1;
uint8_t digit2;
uint8_t digit3;
uint8_t digit4;

uint8_t roll;

// initial life values
uint8_t aLife = 20;
uint8_t bLife = 20;


void setup() {

  pinMode(rollSwitch, INPUT);
  pinMode(resetSwitch, INPUT);
  pinMode(upA, INPUT);
  pinMode(downA, INPUT);
  pinMode(upB, INPUT);
  pinMode(downB, INPUT);

  lc.shutdown(0, false);
  lc.setIntensity(0, 10);
  lc.clearDisplay(0);

  lc.setDigit(0, 0, 0, true);
  lc.setDigit(0, 1, 0, false);

  lc.setDigit(0, 2, 0, false);
  lc.setDigit(0, 3, 0, false);

  updateALife();
  updateBLife();

  Serial.begin(9600);
  while (!Serial) {
    ;
  }

  Entropy.initialize();

}


void loop() {

    potVal = map(analogRead(potPin), 0, 1023, 0, 15); //map analog input
    delay(10);
    lc.setIntensity(0, potVal);
    delay(10);

    upAState = digitalRead(upA);
    downAState = digitalRead(downA);
    upBState = digitalRead(upB);
    downBState = digitalRead(downB);
    rollSwitchState = digitalRead(rollSwitch);
    resetSwitchState = digitalRead(resetSwitch);

    if (rollSwitchState == HIGH)
    {
      whoStarts();
      delay(500);
    }

    if (resetSwitchState == HIGH)
    {
      aLife = 20;
      bLife = 20;
      updateALife();
      updateBLife();
      delay(500);
    }

    if (upAState == HIGH)
    {
      aLife += 1;
      updateALife();
      delay(500);
    }

    if (downAState == HIGH)
    {
      aLife -= 1;
      updateALife();
      delay(500);
    }

    if (upBState == HIGH)
    {
      bLife += 1;
      updateBLife();
      delay(500);
    }

    if(downBState == HIGH)
    {
      bLife -= 1;
      updateBLife();
      delay(500);
    }
}

// returns dice roll result
uint8_t diceRoll() {
  roll = Entropy.random(1,21); // D20
  return roll;
}

// rolls dice for both players and displays results
void whoStarts() {
  uint8_t aRoll = diceRoll();
  uint8_t bRoll = diceRoll();

  digit1 = aRoll/10;
  digit2 = aRoll%10;
  digit3 = bRoll/10;
  digit4 = bRoll%10;

  lc.setDigit(0,0,digit1,false);
  lc.setDigit(0,1,digit2,false);
  lc.setDigit(0,2,digit3,false);
  lc.setDigit(0,3,digit4,false);
}

// updates player A life display 
void updateALife()
  {
  digit1 = aLife/10;
  digit2 = aLife%10;

  lc.setDigit(0,0,digit1,false);
  lc.setDigit(0,1,digit2,false);
}

// updates player B life display
void updateBLife()
  {
  digit3 = bLife/10;
  digit4 = bLife%10;

  lc.setDigit(0,2,digit3,false);
  lc.setDigit(0,3,digit4,false);
}
