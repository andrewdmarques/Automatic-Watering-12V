
#include <Arduino.h>

// ESP32 DevKit V1 - 24-Hour GPIO Timer
// Avoids boot-strapping, flash and USB serial pins.
//
// Startup: all selected pins LOW
// Wait 5 seconds
// All selected pins HIGH simultaneously
// Each pin turns LOW after its assigned duration
// Repeat every 24 hours

const int NUM_PINS = 14;

const int pins[NUM_PINS] = {
  16, 17, 18, 19, 21, 22, 23,
  25, 26, 27, 32, 33, 13, 14
};

const unsigned long durations[NUM_PINS] = {
  15000UL,    // GPIO16 - 15 seconds
  30000UL,    // GPIO17 - 30 seconds
  45000UL,    // GPIO18 - 45 seconds
  60000UL,    // GPIO19 - 60 seconds
  90000UL,    // GPIO21 - 90 seconds
  120000UL,   // GPIO22 - 120 seconds
  180000UL,   // GPIO23 - 3 minutes
  240000UL,   // GPIO25 - 4 minutes
  300000UL,   // GPIO26 - 5 minutes
  360000UL,   // GPIO27 - 6 minutes
  480000UL,   // GPIO32 - 8 minutes
  600000UL,   // GPIO33 - 10 minutes
  720000UL,   // GPIO13 - 12 minutes
  840000UL    // GPIO14 - 14 minutes
};

const unsigned long DAY_MS = 86400000UL;
const unsigned long START_DELAY = 5000UL;

unsigned long cycleStart;
bool pinActive[NUM_PINS];

void setup() {

  // Set selected pins LOW
  for (int i = 0; i < NUM_PINS; i++) {
    digitalWrite(pins[i], LOW);
    pinMode(pins[i], OUTPUT);
    pinActive[i] = false;
  }

  cycleStart = millis();
}

void loop() {

  unsigned long now = millis();
  unsigned long elapsed = now - cycleStart;

  // Advance to next 24-hour cycle
  if (elapsed >= DAY_MS) {
    cycleStart += DAY_MS;
    elapsed = now - cycleStart;
  }

  // Calculate desired pin states
  for (int i = 0; i < NUM_PINS; i++) {

    bool shouldBeHigh = false;

    if (elapsed >= START_DELAY) {
      unsigned long activeTime = elapsed - START_DELAY;

      if (activeTime < durations[i]) {
        shouldBeHigh = true;
      }
    }

    // Change GPIO only when necessary
    if (shouldBeHigh != pinActive[i]) {
      digitalWrite(pins[i], shouldBeHigh ? HIGH : LOW);
      pinActive[i] = shouldBeHigh;
    }
  }

  delay(10);
}
