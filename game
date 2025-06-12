int ledPins[] = {5, 4, 3, 2};           // Array storing pin numbers for the 4 LEDs
int buttonPins[] = {A2, A3, A4, A5};    // Array storing pin numbers for the 4 buttons

int sequence[100];                      // Array to store the sequence of LED indexes
int sequenceLength = 0;                 // Number of steps currently in the sequence
int playerIndex = 0;                    // Current position of player input in the sequence
bool waitingForPlayer = false;          // Flag to indicate if the game is waiting for player's input

void setup() {
  for (int i = 0; i < 4; i++) {         // Loop through each LED/button pair
    pinMode(ledPins[i], OUTPUT);        // Set LED pins as outputs
    pinMode(buttonPins[i], INPUT);      // Set button pins as inputs
  }

  randomSeed(analogRead(A0));           // Initialize random number generator with noise from analog pin A0
  waitForStart();                       // Wait until player presses a button to start
  nextRound();                         // Start the first round by generating sequence and showing it
}

void loop() {
  if (waitingForPlayer) {                // Only check for input if it's the player's turn
    for (int i = 0; i < 4; i++) {       // Check each button
      if (digitalRead(buttonPins[i]) == HIGH) {  // If button i is pressed
        handlePlayerInput(i);            // Process the player's input for button i
        while (digitalRead(buttonPins[i]) == HIGH);  // Wait until button is released (simple debounce)
        delay(100);                     // Short delay to avoid multiple triggers
      }
    }
  }
}

void waitForStart() {
  flashAll(2);                          // Flash all LEDs twice to indicate waiting for start
  while (true) {                       // Infinite loop until a button press is detected
    for (int i = 0; i < 4; i++) {      // Check each button
      if (digitalRead(buttonPins[i]) == HIGH) {   // If button i is pressed
        while (digitalRead(buttonPins[i]) == HIGH); // Wait for button release
        delay(200);                    
        return;                       // Exit the function to start the game
      }
    }
  }
}

void nextRound() {
  sequence[sequenceLength] = random(0, 4);  // Add a random LED index (0-3) to the sequence
  sequenceLength++;                       // Increase the length of the sequence by 1
  showSequence();                        // Display the full sequence by flashing LEDs
  playerIndex = 0;                      // Reset player input position to start of sequence
  waitingForPlayer = true;               // Set flag to start waiting for player input
}

void showSequence() {
  delay(500);                         
  for (int i = 0; i < sequenceLength; i++) { 
    int led = ledPins[sequence[i]];   // Get the pin number of the LED to flash
    digitalWrite(led, HIGH);          // Turn LED on
    delay(700);                       
    digitalWrite(led, LOW);           // Turn LED off
    delay(300);                       
  }
}

void handlePlayerInput(int btnIndex) {
  flashLED(btnIndex);                  // Flash the LED corresponding to the pressed button
  if (btnIndex == sequence[playerIndex]) {  // Check if player's input matches the sequence step
    playerIndex++;                    // Move to next step in sequence
    if (playerIndex == sequenceLength) {  // If entire sequence entered correctly
      waitingForPlayer = false;       // Stop waiting for input temporarily
      delay(500);                    // Short pause before next round
      nextRound();                   // Start next round by adding a new LED to sequence
    }
  } else {
    gameOver();                      // If wrong input, trigger game over sequence
  }
}

void gameOver() {
  flashAll(3);                      // Flash all LEDs three times to indicate game over
  sequenceLength = 0;               // Reset sequence length to zero
  playerIndex = 0;                  // Reset player input index
  waitingForPlayer = false;          // Stop waiting for player input
  waitForStart();                  // Wait for player to start a new game
  nextRound();                     // Start a new game round
}

void flashLED(int index) {
  digitalWrite(ledPins[index], HIGH);  // Turn on LED at given index
  delay(300);                        // Wait for 300 milliseconds
  digitalWrite(ledPins[index], LOW);   // Turn off LED
}

void flashAll(int times) {
  for (int t = 0; t < times; t++) {  // Repeat flashing 'times' number of times
    for (int i = 0; i < 4; i++) {     // Turn all LEDs on
      digitalWrite(ledPins[i], HIGH);
    }
    delay(300);                      
    for (int i = 0; i < 4; i++) {     // Turn all LEDs off
      digitalWrite(ledPins[i], LOW);
    }
    delay(300);                      
  }
}
