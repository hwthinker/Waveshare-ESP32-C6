# Tutorial  Waveshare ESP32-C6

![image-20250128170821198](./assets/image-20250128170821198.png)

| Perangkat | Pin/Alamat | Keterangan                |
| --------- | ---------- | ------------------------- |
| WS2812B   | GPIO8      | LED RGB addressable       |



[TOC]

## Install Board

1. Masuk ke preferences

![](./assets/02.png)

2. Klik Additional Board Manager

![](./assets/03.png)

3. Tambahkan board esp32 kalimat berikut https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

![](./assets/04.png)

4. Pilih Tools -> Board -> Board Manager

![](./assets/05.png)

5. Search ESP32 kemudian klik install

![](./assets/06.png)

6. Pilih Tools -> Board -> ESP32 -> esp32C6 dev Module

![image-20250128023023757](./assets/image-20250128023023757.png)



7. Langkah berikutnya adalah memastikan Arduino mengetahui Kapasitas Flash yang tersedia. ini dilakukan dengan cara mengatur konfigurasi "Flash Size".  Sesuaikan Flash anda (8MB) menggunakan konfigurasi seperti pada gambar berikut

![image-20250128171334831](./assets/image-20250128171334831.png)

8. Setting port serial , untuk windows bisa cek device manager, pastikan anda telah install driver serial usb anda, bila tidak dikenali di windows install driver serial tersebut melalui link https://www.wch-ic.com/search?q=CH343&t=downloads

![image-20250127114719815](./assets/image-20250127114719815.png)



## Contoh Kode Program

### Test WS2812 menggunakan library FASTLED

LED WS2812 adalah jenis LED RGB dengan pengontrol bawaan yang memungkinkan setiap LED dikendalikan secara individual menggunakan satu pin data. Untuk mempermudah pengendalian LED ini, kita dapat menggunakan library **FASTLED** yang menyediakan fungsi-fungsi intuitif untuk membuat efek cahaya yang menarik. Panduan berikut akan membantu Anda menginstal library FASTLED di Arduino IDE dan menguji contoh kode sederhana untuk mengontrol LED WS2812.

#### Langkah-langkah Instalasi Library FASTLED

Pastikan library **FASTLED** telah terinstal di Arduino IDE Anda. Jika belum, ikuti langkah-langkah berikut:

1. Klik ikon **Library Manager** di panel **sidebar** Arduino IDE.
2. Ketik **FastLED** di kolom pencarian.
3. Temukan library **FastLED by Daniel Garcia**.
4. Klik tombol **Install** untuk menginstalnya.



![image-20250111140442247](./assets/image-20250111140442247.png)

#### Menggunakan Contoh Kode

Setelah library terinstal, buat file baru atau buka contoh program di **File -> Examples -> Basics -> BareMinimum**.  kemudain yang harus anda lakukan adalah:

1. Gantikan kode yang ada dengan kode berikut:

```c++
#include "FastLED.h"

#define NUM_LEDS 1       // Jumlah LED yang digunakan
#define DATA_PIN 8      // Pin Data untuk kontrol LED (dapat diganti sesuai kebutuhan: 38, 16, 8, dll)
#define LED_TYPE WS2812  // Tipe LED yang digunakan (WS2812/Neopixel)
#define COLOR_ORDER GRB  // Urutan warna pada LED (Green-Red-Blue)

// Variabel untuk mengontrol kecerahan LED (range: 0-255)
// Semakin besar nilai, semakin terang LED
uint8_t max_bright = 128;  

// Membuat array untuk menyimpan data LED
CRGB leds[NUM_LEDS];     

// Variabel untuk menghitung urutan LED yang menyala
char i = 0;              

void setup() {
    // Inisialisasi LED strip dengan konfigurasi yang telah ditentukan
    LEDS.addLeds<LED_TYPE, DATA_PIN, COLOR_ORDER>(leds, NUM_LEDS);
    
    // Mengatur tingkat kecerahan LED
    FastLED.setBrightness(max_bright);
}

void loop() {
    // Reset counter jika sudah mencapai LED terakhir
    if (i == NUM_LEDS) {
        i = 0;
    }
    
    // Menyalakan LED dengan warna merah
    leds[i] = CRGB::Red;
    
    // Memperbarui tampilan LED
    FastLED.show();
    
    // Jeda 50 milidetik
    delay(50);
    
    // Mematikan LED (warna hitam = LED mati)
    leds[i] = CRGB::Black;
    
    // Memperbarui tampilan LED
    FastLED.show();
    
    // Jeda 50 milidetik
    delay(50);
    
    // Pindah ke LED berikutnya
    i++;
}
```



## Upload program

Bila tampilan seperti ini maka anda harus mengkonfigurasi ESP32 anda agar masuk ke mode bootloader untuk bisa melakukan download

```
- ---esptool.py v3.0-dev
- ---Serial port COM…
- ---Connecting........_____....._____.....__
```

Langkah yang harus dilakukan

- Tekan dan tahan tombol Boot/0  
- Klik(tekan dan lepas) tombol reset/EN sambil tetap tekan tombol Boot .
- Lepas tombol boot
- Klik tombol upload pada Arduino IDE, bila sukses akan menampilkan info

```cpp
- ---Compressed 261792 bytes to 122378...
- ---Writing at 0x00010000... (12 %)
- ---Writing at 0x00014000... (25 %)
- ---Writing at 0x00018000... (37 %)
```

- Setelah selesai Wajib klik tombol **reset** sekali lagi untuk berpindah dari mode download menjadi mode run

> [!NOTE]  
> INGAT YA WAJIB Di Klik Tombol RESET setelah proses upload selesai, tanpa itu program yang baru diupload tidak akan dijalankan



## Menggunakan library Adafruit

Library **Adafruit NeoPixel** adalah pilihan lain yang populer untuk mengontrol LED WS2812 dan jenis LED serupa. Library ini menyediakan fungsi yang mudah digunakan untuk mengontrol warna dan efek pencahayaan pada strip atau matriks LED. Berikut adalah panduan singkat untuk menginstal library ini dan contoh kode sederhana untuk mengontrol LED WS2812.

#### Langkah-langkah Instalasi Library Adafruit NeoPixel

1. Buka Arduino IDE, lalu klik ikon **Library Manager** di panel **sidebar**.
2. Ketik **Adafruit NeoPixel** di kolom pencarian.
3. Temukan library **Adafruit NeoPixel by Adafruit**.
4. Klik tombol **Install** untuk memasangnya, lebih jelas lihat gambar berikut

![image-20250111143415429](./assets/image-20250111143415429.png)

#### Contoh Kode Mengontrol WS2812

Gunakan kode berikut untuk mengontrol satu LED WS2812 yang menyala berwarna merah selama 1 detik:

```c++
#include <Adafruit_NeoPixel.h>

#define PIN        8 
#define NUMPIXELS  1

Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
  pixels.begin();
}

void loop() {
  pixels.setPixelColor(0, pixels.Color(255,0,0)); 
  pixels.show();
  delay(1000);
  pixels.setPixelColor(0, pixels.Color(0,0,0)); 
  pixels.show();
  delay(1000);
}
```

5. Bila sudah selesai upload program dan kemudian lihat hasilnya

6. Bila berhasil LED RGB WS2812 akan LED akan berkedip  dengan nyala warna merah saat ON

**Catatan:**

- Ganti warna LED dengan nilai RGB yang diinginkan menggunakan `pixels.Color(red, green, blue)`.



## Akses Serial Port di ESP32

Board **ESP32-C6 ** ini mendukung upload dan komunikasi data serial ke komputer dengan dua Mode:

- Mode  USB Serial konverter ch343
- Mode Native USB

yang menarik adalah  terdapat USB hub controller CH334F yang memungkina 2 Serial komunikasi  yaitu USB CH343 dan USB Native terhubung dalam 1 konektor USB. gambarannya bisa lihat pada gambar dibawah ini

![image-20250128173131001](./assets/image-20250128173131001.png)

Bila di cek dilinux akan ada 2 serial sebaga berrikut:

![image-20250128173540234](./assets/image-20250128173540234.png)

### Mode USB Serial konverter (default manufaktur)

Pada mode ini Komunikasi Serial ditangani oleh chip usb serial CH343.  Pastikan juga data serial  di arahkan ke Serial USB CH343 ini  dilakukan dengan menggunakan command line bila dilinux sebagai berikut:

```
udevadm info --name=/dev/ttyACM0
```

Hasilnya bisa di komparasi dengan perintah `lsusb` , dari gambar dibawah nampak /dev/ttyACM0 terhubung dengan usb serial ch343

![image-20250128174713254](./assets/image-20250128174713254.png)

Berikutnya  mengarahkan output serial ke USB serial ch343 dengan cara memilih port  yang benar  sebagai berikut

![image-20250128182752645](./assets/image-20250128182752645.png)

Berikutnya adalah  mendisable  opsi "USB CDC on boot" supaya serial aktif diarahkan ke Serial cha343. Langkah mendisable USB CDC on boot ini bisa di cek pada gambar dibawah ini dengan cara Pliih menu Tools -> USB CD on boot -> Disabled

![image-20250128183040253](./assets/image-20250128183040253.png)

- Jalankan kode sederhana ini

```c++
void setup() {
  // initialize both serial ports:
  Serial.begin(9600);
  while(!Serial);
}

void loop() {
  Serial.println("hello world");
  delay(500);
}
```

- Klik **Verify** untuk memeriksa kode. 
- Klik **Upload** untuk mengunggah kode ke ESP32-C6.
- Bila sudah selesai Silahkan buka Tools -> Serial Monitor
- Atur baud rate di pojok kanan bawah menjadi **9600** (sesuai dengan `Serial.begin(9600)` di kode).
- Anda akan melihat teks **"hello world"** muncul setiap 500ms.

![image-20250112092642208](./assets/image-20250112092642208.png)

### Mode Native USB untuk Upload dan komunikasi data

Pada mode ini Komunikasi Serial ditangani oleh native USB yang merupakan fitur yang ada di ESP32-C6. Untuk mengetahui port mana yang menggunakan native usb bisa dilakukan cara sebelumnya. Untuk pengaturan konfigurasi software pastikan data serial  di arahkan Native USB sebagai berikut:
![image-20250128182543269](./assets/image-20250128182543269.png)

Berikutnya pastikan konfigurasi  USB CDC on Boot  enablekan.  Pengaturan ini memastikan board dapat menggunakan port USB internal untuk komunikasi serial. pilih Tools -> USB CDC on Boot -> Enabled

![image-20250127132458286](./assets/image-20250127132458286.png)



- Jalankan kode sederhana ini

```c++
void setup() {
  // initialize both serial ports:
  Serial.begin(9600);
  while(!Serial);
}

void loop() {
  Serial.println("hello world");
  delay(500);
}
```

- Klik **Verify** untuk memeriksa kode. 
- Klik **Upload** untuk mengunggah kode ke ESP32-C6.
- Bila sudah selesai Silahkan buka Tools -> Serial Monitor
- Atur baud rate di pojok kanan bawah menjadi **9600** (sesuai dengan `Serial.begin(9600)` di kode).
- Anda akan melihat teks **"hello world"** muncul setiap 500ms.

![image-20250112092642208](./assets/image-20250112092642208.png)



## Pemecahan Masalah

### A. Port Com  tidak dapat dikenali di Arduino

Bila port COM di windows tidak dikenali pastikan install driver terlebih dahulu ada  beberapa kemungkinan yaitu permasalaha di chip, permasalah di usb serial, permasalahan di kabe dan permasalahan di komputer. 

### 1. Belum install  driver USB

Permasalahan yang umum adalah berlum install driver usb serial. driver usb serial di board ini menggunakan driver ch343 yang dapat diunduh melalui google dengan kata kunci "download driver ch343" atau bisa juga langsung ke situs manufakturnya. 

### 2. Permasalahan kabel USB
Bila masih belum dikenali kemungkinan Kabel USB bukan merupakan kabel data melainkan kabel power saja. Jadi pastikan menggunakan kabel data yang berkualitas yang pendek (maksimal 1m). bila tidak dikenali juga coba di pindah port usb lainnya di komputer tersebut atau menggunakan komputer lainnya.  

### 3. Belum masuk Mode Bootloader

Belum masuk mode bootloader juga menyebebkan komputer tidak mengenali device ESP32. Untuk mengatasi ini caranya cukup simple yaitu dengan menekan tombol tertentu yang ada di ESP32 sehingga bisa masuk mode bootloader. untuk masuk ke mode bootloader  caranya sebagai berikut:

- Tekan dan tahan tombol Boot/0  
- Klik(tekan dan lepas) tombol reset/EN sambil tetap tekan tombol Boot .
- Lepas tombol boot
- Setelah selesai Wajib klik tombol **reset** sekali lagi untuk berpindah dari mode download menjadi mode run

### B. Program tidak dapat berjalan setelah diunggah

Setelah upload berhasil, Anda perlu menekan tombol Reset sebelum dapat dijalankan.

### Referensi

-  [YD-ESP32-C6-SCH.pdf](document/YD-ESP32-C6-SCH.pdf) 
