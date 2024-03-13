#include <DHT.h>
#include <Servo.h>

#define DHTPIN D2       // Pin tempat sambungan output sensor DHT
#define DHTTYPE DHT11   // Tipe sensor DHT yang digunakan (DHT11 atau DHT22)

DHT dht(DHTPIN, DHTTYPE);
Servo myservo;  // Membuat objek servo untuk mengendalikan servo motor

const int waterLevelPin = A0;  // Pin tempat sambungan sensor water level
const int buzzerPin = D3;       // Pin tempat sambungan buzzer
const int servoPin = D1;        // Pin tempat sambungan servo motor

void setup() {
  Serial.begin(9600);
  pinMode(waterLevelPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  dht.begin();
  myservo.attach(servoPin);  // Menghubungkan servo motor ke pin D6
}

void loop() {
  // Membaca tingkat air dari sensor
  int waterLevel = analogRead(waterLevelPin);
  
  // Menampilkan tingkat air ke Serial Monitor
  Serial.print("Water Level: ");
  Serial.println(waterLevel);
  
  // Mengecek tingkat air
  if (waterLevel > 250) {  // Atur batas sesuai dengan kebutuhan Anda
    // Jika tingkat air melebihi batas, bunyikan buzzer
    digitalWrite(buzzerPin, HIGH);
    // Gerakkan servo motor ke posisi tertentu
    myservo.write(90);  // Contoh: menggerakkan servo ke posisi 90 derajat
  } else {
    digitalWrite(buzzerPin, LOW);
    // Gerakkan servo motor ke posisi lain jika diperlukan
    myservo.write(10);
  }
  
  // Membaca suhu dan kelembaban
  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  // Menampilkan suhu dan kelembaban ke Serial Monitor
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.print(" %\t");
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.println(" *C");

  delay(2000);  // Menunda pembacaan selama 2 detik
}
