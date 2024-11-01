# RDD CAR Project

// Define pins for the ultrasonic sensor
const int trigPin = 9;
const int echoPin = 8;

// Define motor pins connected to the L293D
const int motorPin1 = 5;
const int motorPin2 = 6;
const int enablePin = 3; // PWM pin for controlling speed

long duration;
int distance;

void setup() {
  // Setup the serial monitor for debugging
  Serial.begin(9600);

  // Define ultrasonic sensor pins as input/output
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  // Define motor pins as output
  pinMode(motorPin1, OUTPUT);
  pinMode(motorPin2, OUTPUT);
  pinMode(enablePin, OUTPUT);

  // Initialize the motor to stop
  digitalWrite(motorPin1, LOW);
  digitalWrite(motorPin2, LOW);
  analogWrite(enablePin, 0);
}

void loop() {
  // Clear the trigger pin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  // Trigger the ultrasonic sensor
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read the echo pin for the duration of the pulse
  duration = pulseIn(echoPin, HIGH);

  // Calculate the distance in cm
  distance = duration * 0.034 / 2;

  // Print the distance to the serial monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Control the motor based on the distance
  if (distance < 20) {
    // If object is too close, stop the motor
    digitalWrite(motorPin1, LOW);
    digitalWrite(motorPin2, LOW);
    analogWrite(enablePin, 0); // Stop motor
  } else {
    // If object is far, run the motor
    digitalWrite(motorPin1, HIGH);
    digitalWrite(motorPin2, LOW);
    analogWrite(enablePin, 200); // Set motor speed
  }

  // Small delay before the next measurement
  delay(100);
}
