# Switching-light-IOT-prg

#include <LiquidCrystal.h>

LiquidCrystal lcd(2, 3, 4, 5, 6, 7);
#define e_s1 9 // echo pin for first sensor
#define t_s1 10 // trigger pin for first sensor
#define e_s2 11 // echo pin for second sensor
#define t_s2 12 // trigger pin for second sensor
int led = 8;
float duration_us1, distance_cm1, duration_us2, distance_cm2;

void setup() {
  Serial.begin(9600);
  pinMode(t_s1, OUTPUT);
  pinMode(e_s1, INPUT);
  pinMode(t_s2, OUTPUT);
  pinMode(e_s2, INPUT);
  pinMode(led, OUTPUT);
  lcd.begin(16, 2);
  lcd.setCursor(0, 0);
lcd.print("     Welcome    ");
delay(1000); // Waiting for a while
lcd.clear(); 
}

void loop() {
  digitalWrite(t_s1, HIGH);
  delayMicroseconds(10);
  digitalWrite(t_s1, LOW);
  duration_us1 = pulseIn(e_s1, HIGH);
  distance_cm1 = 0.017 * duration_us1;

  digitalWrite(t_s2, HIGH);
  delayMicroseconds(10);
  digitalWrite(t_s2, LOW);
  duration_us2 = pulseIn(e_s2, HIGH);
  distance_cm2 = 0.017 * duration_us2;

  if (distance_cm1 <= 10) {
    digitalWrite(led, LOW);
    lcd.setCursor(0, 0);
    lcd.print(" Light is ON ");
  }
  else if (distance_cm2 <= 10) {
    digitalWrite(led, HIGH);
    lcd.setCursor(0, 0);
    lcd.print(" Light is OFF ");
  }
}
