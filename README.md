# Test-Soul-Parking

//library sensor dht11 yang digunakan
#include "DHT.h"

//pin yang digunakan
//dht11 = 2, led = 3, buzzer = 4
#define DHTPIN 2
int led = 3;
int buzzer = 4;

//tipe data yang digunakan sensor dht adalah float
// i = untuk suhu , j = untuk humidity
float i = 0;
float j = 0;

//max suhu untuk menyalakan led, apabila suhu diatas 35 C led dan buzzer akan menyala dan berbunyi
float max_suhu = 35;


#define DHTTYPE DHT11
DHT dht (DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  
  pinMode(led , OUTPUT);
  pinMode(buzzer, OUTPUT);
  
  dht.begin();
  

}
//disini saya buat void sensor dht11 terpisah sehingga di void loop hanya akan saya panggil saja

void loop() {
  ceksuhu();
  ledbuzzer();

}

void ceksuhu(){
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  if (isnan (h) || isnan (t)){
    Serial.println("Pembacaan Sensor DHT11 Gagal ");
    return;
  }
  Serial.print("kelembapan:  ");
  Serial.print(h);
  Serial.print(" %\t ");
  Serial.print("suhu :");
  Serial.print(t);
  Serial.println(" *C ");
  //variable output dari dht11 disimpan ke variabel lain untuk join ke void yang lain
  i = t;
  j = h;
  //
  delay(500);
}

void ledbuzzer(){
  if ( i >= max_suhu ){
    digitalWrite(led, HIGH);
    digitalWrite(buzzer, HIGH);
    Serial.println("suhu diatas batas 35 *C");
  }else if( i <= max_suhu){
    digitalWrite(led, LOW);
    digitalWrite(buzzer, LOW);
    Serial.println("suhu dibawah batas 35 *C");
  }
  delay(500);
}
