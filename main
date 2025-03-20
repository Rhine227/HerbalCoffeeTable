///////////////////////////////////////////////////////////////////////////////////////////////////
//                                 Herbal Coffee Table                                           //
///////////////////////////////////////////////////////////////////////////////////////////////////

#include <AccelStepper.h> // Library to control stepper motors

// Define pins for stepper motors and limit switches
#define BOWL_STEP_PIN 2 // Pin for step signal of the bowl stepper motor
#define BOWL_DIR_PIN 3  // Pin for direction signal of the bowl stepper motor
#define AUGER_STEP_PIN 4 // Pin for step signal of the auger stepper motor
#define AUGER_DIR_PIN 5  // Pin for direction signal of the auger stepper motor
#define LOAD_POSITION_LIMIT_SWITCH_PIN 6 // Pin for the load position limit switch
#define HIT_LIMIT_SWITCH_PIN 7 // Pin for the hit position limit switch

// Define pin for HIT VAC MOTOR
#define HIT_VAC_MOTOR_PIN 8 // Pin to control the vacuum motor for the "HIT" process

// Define touchscreen button states
#define LOAD_BUTTON_PIN 9  // Pin for the "LOAD" button
#define HIT_BUTTON_PIN 10  // Pin for the "HIT" button
#define CLEAN_BUTTON_PIN 11 // Pin for the "CLEAN" button

// Create stepper motor instances
// The AccelStepper library is used to control stepper motors with acceleration and deceleration
AccelStepper bowlStepper(AccelStepper::DRIVER, BOWL_STEP_PIN, BOWL_DIR_PIN); // Bowl stepper motor
AccelStepper augerStepper(AccelStepper::DRIVER, AUGER_STEP_PIN, AUGER_DIR_PIN); // Auger stepper motor

void setup() {
  // Initialize pins for limit switches and buttons as input with pull-up resistors
  pinMode(LOAD_POSITION_LIMIT_SWITCH_PIN, INPUT_PULLUP); // Load position limit switch
  pinMode(HIT_LIMIT_SWITCH_PIN, INPUT_PULLUP); // Hit position limit switch
  pinMode(HIT_VAC_MOTOR_PIN, OUTPUT); // Initialize vacuum motor pin as output
  pinMode(LOAD_BUTTON_PIN, INPUT_PULLUP); // Load button
  pinMode(HIT_BUTTON_PIN, INPUT_PULLUP); // Hit button
  pinMode(CLEAN_BUTTON_PIN, INPUT_PULLUP); // Clean button

  // Configure stepper motors with maximum speed and acceleration
  bowlStepper.setMaxSpeed(1000); // Maximum speed for the bowl stepper motor
  bowlStepper.setAcceleration(500); // Acceleration for the bowl stepper motor
  augerStepper.setMaxSpeed(1000); // Maximum speed for the auger stepper motor
  augerStepper.setAcceleration(500); // Acceleration for the auger stepper motor
}

void loop() {
  // Check if the "LOAD" button is pressed
  if (digitalRead(LOAD_BUTTON_PIN) == LOW) {
    // Rotate the bowl stepper motor clockwise until the load position limit switch is triggered
    bowlStepper.setSpeed(500); // Set speed for clockwise rotation
    while (digitalRead(LOAD_POSITION_LIMIT_SWITCH_PIN) == HIGH) { // Wait until the limit switch is pressed
      bowlStepper.runSpeed(); // Keep rotating the motor
    }
    bowlStepper.stop(); // Stop the motor when the limit switch is triggered

    // Rotate the auger stepper motor for 2 full rotations
    augerStepper.move(2 * 200); // Assuming 200 steps per revolution
    while (augerStepper.distanceToGo() != 0) { // Wait until the motor completes the movement
      augerStepper.run(); // Keep rotating the motor
    }

    // Rotate the bowl stepper motor counter-clockwise until the hit position limit switch is triggered
    bowlStepper.setSpeed(-500); // Set speed for counter-clockwise rotation
    while (digitalRead(HIT_LIMIT_SWITCH_PIN) == HIGH) { // Wait until the limit switch is pressed
      bowlStepper.runSpeed(); // Keep rotating the motor
    }
    bowlStepper.stop(); // Stop the motor when the limit switch is triggered
  }

  // Check if the "HIT" button is pressed
  if (digitalRead(HIT_BUTTON_PIN) == LOW) {
    // Run the vacuum motor as long as the hit limit switch is engaged and the load position limit switch is not engaged
    while (digitalRead(HIT_LIMIT_SWITCH_PIN) == LOW && digitalRead(LOAD_POSITION_LIMIT_SWITCH_PIN) == HIGH) {
      digitalWrite(HIT_VAC_MOTOR_PIN, HIGH); // Turn on the vacuum motor
    }
    digitalWrite(HIT_VAC_MOTOR_PIN, LOW); // Turn off the vacuum motor when the condition is no longer met
  }

  // Check if the "CLEAN" button is pressed
  if (digitalRead(CLEAN_BUTTON_PIN) == LOW) {
    // Pseudo-code for the cleaning process
    // 1. Rotate the bowl stepper motor to a specific cleaning position
    // 2. Activate the cleaning mechanism (e.g., spray water or cleaning solution)
    // 3. Wait for a specified duration
    // 4. Rotate the bowl stepper motor back to the default position
    // 5. Deactivate the cleaning mechanism
    // Note: The actual implementation of the cleaning process depends on the hardware setup
  }
}
