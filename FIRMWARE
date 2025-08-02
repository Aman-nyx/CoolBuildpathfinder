
  // Button pins
  int but1 = 1;
int but2 = 2;
int but3 = 3;
int but4 = 4;

// LED pins
int led1 = 10;
int led2 = 9;
int led3 = 5;
int led4 = 6;

bool gameRunning = false;
bool simonPlaying = false;
bool simonMode = false;
int currentLED = -1;
bool bombExploded = false;
unsigned long explosionStartTime = 0;
unsigned long lightOnTime = 0;
unsigned long timeLimit = 1500;
unsigned long bombStartTime = 0;
unsigned long bombDuration = 0;
bool bombTimerRunning = false;

#define MAX_SEQUENCE_LENGTH 50
int simonSequence[MAX_SEQUENCE_LENGTH];
int simonLength = 1;
int simonIndex = 0;
void startGame();
void setup() {
  pinMode(but1, INPUT_PULLUP);
  pinMode(but2, INPUT_PULLUP);
  pinMode(but3, INPUT_PULLUP);
  pinMode(but4, INPUT_PULLUP);

  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
  pinMode(led3, OUTPUT);
  pinMode(led4, OUTPUT);

  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
}

// ... all your setup and global variables remain unchanged

void loop() {
  static unsigned long pressTime1 = 0;
  static unsigned long pressTime2 = 0;
  static unsigned long pressTime3 = 0;
  static unsigned long pressTime4 = 0;

  if (simonMode) {
    simonCheck();
    return;
  }

  if (digitalRead(but1) == LOW) {
    if (pressTime1 == 0) pressTime1 = millis();
    if (millis() - pressTime1 > 1500 && !gameRunning && !simonPlaying) {
      startGame();
      pressTime1 = 0;
    }
  } else {
    pressTime1 = 0;
  }

  if (digitalRead(but2) == LOW) {
    if (pressTime2 == 0) pressTime2 = millis();
    if (millis() - pressTime2 > 1500 && !gameRunning && !simonPlaying) {
      startSimonGame();
      pressTime2 = 0;
    }
  } else {
    pressTime2 = 0;
  }

  if (digitalRead(but3) == LOW) {
    if (pressTime3 == 0) pressTime3 = millis();
    if (millis() - pressTime3 > 1500 && !gameRunning && !simonPlaying && !bombTimerRunning) {
      startBombTimer(15000);  // 15 seconds
      pressTime3 = 0;
    }
  } else {
    pressTime3 = 0;
  }

  if (digitalRead(but4) == LOW) {
    if (pressTime4 == 0) pressTime4 = millis();
    if (millis() - pressTime4 > 1500 && !gameRunning && !simonPlaying && !bombTimerRunning) {
      startBombTimer(30000);  // 30 seconds
      pressTime4 = 0;
    }
  } else {
    pressTime4 = 0;
  }

  if (gameRunning) {
    if (millis() - lightOnTime > timeLimit) {
      gameOver();
    }

    if (digitalRead(but1) == LOW) {
      if (currentLED == 0) {
        delay(100);
        correctHit();
      } else {
        delay(100);
        gameOver();
      }
    }
    if (digitalRead(but2) == LOW) {
      if (currentLED == 1) {
        delay(100);
        correctHit();
      } else {
        delay(100);
        gameOver();
      }
    }
    if (digitalRead(but3) == LOW) {
      if (currentLED == 2) {
        delay(100);
        correctHit();
      } else {
        delay(100);
        gameOver();
      }
    }
    if (digitalRead(but4) == LOW) {
      if (currentLED == 3) {
        delay(100);
        correctHit();
      } else {
        delay(100);
        gameOver();
      }
    }
  }

  if (simonPlaying) {
    if (digitalRead(but1) == LOW) simonCheck();
    if (digitalRead(but2) == LOW) simonCheck();
    if (digitalRead(but3) == LOW) simonCheck();
    if (digitalRead(but4) == LOW) simonCheck();
  }

  if (bombTimerRunning) {
    runBombTimer();
  }
}


void nextRound() {
  currentLED = random(0, 4);
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);

  if (currentLED == 0) digitalWrite(led1, HIGH);
  else if (currentLED == 1) digitalWrite(led2, HIGH);
  else if (currentLED == 2) digitalWrite(led3, HIGH);
  else if (currentLED == 3) digitalWrite(led4, HIGH);

  lightOnTime = millis();
}

void correctHit() {
  flashAllLEDs(2500);
  if (timeLimit > 400) timeLimit -= 100;
  nextRound();
}

void gameOver() {
  for (int i = 0; i < 10; i++) {
    digitalWrite(led1, HIGH);
    digitalWrite(led2, HIGH);
    digitalWrite(led3, HIGH);
    digitalWrite(led4, HIGH);
    delay(250);
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    delay(250);
  }
  gameRunning = false;
  simonPlaying = false;
  simonMode = false;
}

void flashAllLEDs(int duration) {
  unsigned long start = millis();
  while (millis() - start < duration) {
    digitalWrite(led1, HIGH);
    digitalWrite(led2, HIGH);
    digitalWrite(led3, HIGH);
    digitalWrite(led4, HIGH);
    delay(100);
    digitalWrite(led1, LOW);
    digitalWrite(led2, LOW);
    digitalWrite(led3, LOW);
    digitalWrite(led4, LOW);
    delay(100);
  }
}

void startSimonGame() {
  simonMode = true;
  gameRunning = true;
  simonLength = 1;
  delay(500);
  generateSimonSequence();
  showSimonSequence();
}

void generateSimonSequence() {
  for (int i = 0; i < MAX_SEQUENCE_LENGTH; i++) {
    simonSequence[i] = random(0, 4);
  }
}

void showSimonSequence() {
  for (int i = 0; i < simonLength; i++) {
    lightLED(simonSequence[i]);
    delay(500);
    turnOffAllLEDs();
    delay(200);
  }
  simonIndex = 0;
}

void simonCheck() {
  int pressed = -1;

  if (digitalRead(but1) == LOW) pressed = 0;
  if (digitalRead(but2) == LOW) pressed = 1;
  if (digitalRead(but3) == LOW) pressed = 2;
  if (digitalRead(but4) == LOW) pressed = 3;

  if (pressed != -1) {
    delay(150);
    if (pressed == simonSequence[simonIndex]) {
      simonIndex++;
      if (simonIndex == simonLength) {
        flashAllLEDs(500);
        simonLength++;
        delay(500);
        showSimonSequence();
      }
    } else {
      gameOver();
    }

    while (digitalRead(but1) == LOW || digitalRead(but2) == LOW || digitalRead(but3) == LOW || digitalRead(but4) == LOW) {
      delay(10);
    }
  }
}

void lightLED(int ledNum) {
  digitalWrite(led1, ledNum == 0 ? HIGH : LOW);
  digitalWrite(led2, ledNum == 1 ? HIGH : LOW);
  digitalWrite(led3, ledNum == 2 ? HIGH : LOW);
  digitalWrite(led4, ledNum == 3 ? HIGH : LOW);
}

void turnOffAllLEDs() {
  digitalWrite(led1, LOW);
  digitalWrite(led2, LOW);
  digitalWrite(led3, LOW);
  digitalWrite(led4, LOW);
}


void runBombTimer() {
  if (bombExploded) {
    if (millis() - explosionStartTime >= 40000) {
      bombExploded = false;
      bombTimerRunning = false;
      turnOffAllLEDs();
    } else {

      static unsigned long lastFlash = 0;
      static bool flashState = false;
      if (millis() - lastFlash >= 100) {
        flashState = !flashState;
        digitalWrite(led1, flashState ? HIGH : LOW);
        digitalWrite(led2, flashState ? HIGH : LOW);
        digitalWrite(led3, flashState ? HIGH : LOW);
        digitalWrite(led4, flashState ? HIGH : LOW);
        lastFlash = millis();
      }
    }
    return;
  }

  unsigned long elapsed = millis() - bombStartTime;
  if (elapsed >= bombDuration) {
    bombExploded = true;
    explosionStartTime = millis();
    return;
  }

  float progress = (float)elapsed / bombDuration;
  int flashSpeed = 800 - int(progress * 700);

  static unsigned long lastToggleTime = 0;
  static bool ledsOn = false;

  if (millis() - lastToggleTime >= flashSpeed) {
    ledsOn = !ledsOn;
    digitalWrite(led1, ledsOn ? HIGH : LOW);
    digitalWrite(led2, ledsOn ? HIGH : LOW);
    digitalWrite(led3, ledsOn ? HIGH : LOW);
    digitalWrite(led4, ledsOn ? HIGH : LOW);
    lastToggleTime = millis();
  }
}
void startBombTimer(unsigned long duration) {
  bombStartTime = millis();
  bombDuration = duration;
  bombTimerRunning = true;
  bombExploded = false;
}
void startGame() {
  gameRunning = true;
  timeLimit = 1500;
  nextRound();
}
