// Pin definitions
const int redPin = 13;
const int yellowPin = 12;
const int greenPin = 11;

const int pedRedPin = 10;
const int pedGreenPin = 9;
const int pedButtonPin = 8;

// declaring variables
const int delay_time = 10000;
const int yellow_delay_time = delay_time / 5;
const int flashDelay = delay_time / 10;

bool pedRequest = false;
enum State {GREEN, RED, YELLOW, RED_YELLOW, PED_CROSS};
State currentState = RED;

// Flash pedestrian green light to indicate the effects of pressing the button
void flashPedGreen(int flashes) {
  for (int i = 0; i < flashes; i++) {
    digitalWrite(pedGreenPin, HIGH);
    delay(flashDelay);
    digitalWrite(pedGreenPin, LOW);
    delay(flashDelay);
  }
}

// Checking if the button is been pressed
void updatePedLights() {
  if (digitalRead(pedButtonPin) == LOW) {
    // delay(yellow_delay_time);
    pedRequest = true;
    currentState = PED_CROSS;
  }
}

void setup() 
{
  // Setting the seven segment display pin states
  for (int i=0; i<7; i++)
  {
    pinMode(i,OUTPUT);
  }
  
  // Setting pin states
  pinMode(redPin, OUTPUT);
  pinMode(yellowPin, OUTPUT);
  pinMode(greenPin, OUTPUT);

  pinMode(pedRedPin, OUTPUT);
  pinMode(pedGreenPin, OUTPUT);
  pinMode(pedButtonPin, INPUT_PULLUP);

  // Initial state
  digitalWrite(yellowPin, LOW);
  digitalWrite(redPin, LOW);
  digitalWrite(greenPin, HIGH);
  digitalWrite(pedRedPin, HIGH);
  digitalWrite(pedGreenPin, LOW);
}

// The count down for the seven segment display
void showDigit(int digit)
{
  const byte segments[] = {
    0b1101111,
    0b1111111,
    0b0111,
    0b1111101,
    0b1101101,
    0b1100110,
    0b1001111,
    0b1011011,
    0b110,
    0b0111111,
    0b0000000
  };
  
  byte current = segments[digit];
   for (int i=0; i<7; i++)
   {
    digitalWrite(i, current &1);
    current >>= 1; 
   }
}

// Count down
void countDown() {
  for (int i=0; i<=10; i++){
    if (i <= 5){
      showDigit(i);
      delay(1000);
    }
    else{
      digitalWrite(yellowPin, HIGH);
      showDigit(i);
      delay(1000);
    }
  }
}

void loop() {
  updatePedLights();
  switch (currentState) {
    case PED_CROSS:
      if (pedRequest) {
        digitalWrite(redPin, HIGH);
        digitalWrite(yellowPin, LOW);
        digitalWrite(greenPin, LOW);
        digitalWrite(pedRedPin, LOW);
        digitalWrite(pedGreenPin, HIGH);
        flashPedGreen(6);
        pedRequest = false;
        currentState = RED_YELLOW;
      }
      break;

    case RED_YELLOW:
      updatePedLights();
      if (pedRequest == false){
      digitalWrite(redPin, HIGH);
      digitalWrite(yellowPin, HIGH);
      digitalWrite(greenPin, LOW);
      digitalWrite(pedRedPin, LOW);
      digitalWrite(pedGreenPin, HIGH);
      }
      updatePedLights();
      if (pedRequest == false){
        delay(yellow_delay_time);
        currentState = GREEN;
      }
      break;
      
    case GREEN:
      updatePedLights();
      if (pedRequest == false){
      digitalWrite(redPin, LOW);
      digitalWrite(yellowPin, LOW);
      digitalWrite(greenPin, HIGH);
      digitalWrite(pedRedPin, HIGH);
      digitalWrite(pedGreenPin, LOW);
      }
      countDown();
      updatePedLights();
      if (pedRequest == false){
        currentState = RED;
      }
      break;  

    case RED:
      updatePedLights();
      if (pedRequest == false){
        digitalWrite(redPin, HIGH);
        digitalWrite(yellowPin, LOW);
        digitalWrite(greenPin, LOW);
        digitalWrite(pedRedPin, LOW);
        digitalWrite(pedGreenPin, HIGH);
      }
      updatePedLights();
      if (pedRequest == false){
        delay(delay_time);
        currentState = RED_YELLOW;
        }
      break;

    default:
      digitalWrite(redPin, LOW);
      digitalWrite(yellowPin, LOW);
      digitalWrite(greenPin, HIGH);
      digitalWrite(pedRedPin, HIGH);
      digitalWrite(pedGreenPin, LOW);
      delay(delay_time);
      currentState = RED;
      break;
  }
}
