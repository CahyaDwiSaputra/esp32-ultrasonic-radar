# 📡 ESP32 Ultrasonic Distance Radar & Proximity Alert

Sistem pendeteksi jarak rintangan berbasis mikrokontroler **ESP32** menggunakan sensor ultrasonik **HC-SR04** dengan visualisasi serial monitor dan indikator peringatan LED otomatis ketika objek mendekat di bawah 20 cm.

---

## 🚀 Fitur Utama
- **Pengukuran Jarak Real-Time**: Menghitung waktu pantulan gelombang ultrasonik (*Time of Flight*) dan mengonversinya menjadi jarak centimeter:
  $$d = \frac{t \times 0.034}{2}$$
- **Threshold Alert Proximity**: Mengaktifkan pin LED secara instan saat rintangan terdeteksi pada jarak $< 20\text{ cm}$.
- **Telemetri Serial**: Mengirim data jarak berkala dengan baudrate `115200` bps untuk debugging dan integrasi lanjutan.
- **Dilengkapi Dokumentasi Praktikum**: PDF laporan praktikum & simulasi Wokwi di folder `docs/`.

---

## 🛠️ Skema Rangkaian & Pinout (ESP32 DOIT DevKit V1)
| Komponen | Pin Sensor / Modul | Pin ESP32 (GPIO) | Keterangan |
|---|---|---|---|
| **HC-SR04** | VCC | 5V / VIN | Catu daya sensor |
| **HC-SR04** | GND | GND | Ground |
| **HC-SR04** | TRIG | **GPIO 5** | Output pulsa trigger trigger 10µs |
| **HC-SR04** | ECHO | **GPIO 18** | Input pembacaan durasi pulsa |
| **LED Alert** | Anoda (+) | **GPIO 2** | Melalui resistor 220Ω |
| **LED Alert** | Katoda (-) | GND | Ground |

---

## 💻 Kode Utama (`esp32_ultrasonic_radar.ino`)
```cpp
#define TRIG_PIN 5       
#define ECHO_PIN 18      
#define LED_PIN 2        

long duration;
float distance;

void setup() {
  Serial.begin(115200);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  duration = pulseIn(ECHO_PIN, HIGH);
  distance = (duration * 0.034) / 2;

  Serial.print("Jarak: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance < 20) {
    digitalWrite(LED_PIN, HIGH);  
  } else {
    digitalWrite(LED_PIN, LOW);   
  }

  delay(500); 
}
```

---

## 👤 Author
- **Cahya Dwi Saputra**
