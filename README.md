# Raspberry-pi-Project-007_019_052

#### Nama Anggota
| No. | Nama                                    | NRP         | 
|-----|-----------------------------------------|-------------|
| 1   | Revalina Erica Permatasari              | 5027241007  | 
| 2   | Syifa Nurul Alfiah                      | 5027241019  | 
| 3   | Salsa Bil Ulla                          | 5027241052  | 

## Langkah-Langkah Pengerjaan Projek Raspberry Pi Dengan Sensor Jarak HC-SR04
### Alat yang Digunakan
| No. | Nama Barang / Alat                      | 
|-----|-----------------------------------------|
| 1   | Raspberry Pi3 Pi 3 Model b+ 3b+         | 
| 2   | Sensor Ultrasonik HC-SR04               |
| 3   | Kabel Jumper                            |
| 1   | Aplikasi terminal                       |

### Skema Koneksi
![WhatsApp Image 2025-10-15 at 11 59 56_fbbbb0d2](https://github.com/user-attachments/assets/7df2f183-32b6-44e4-91e7-31dfdd172140)

| Pin HC-SR04 | Pin Raspberry Pi | Keterangan                      | 
|-------------|------------------|---------------------------------|
| VCC         | Pin 2 (5V)       | Power 5V                        | 
| GND         | Pin 6 (GND)      | Ground                          | 
| TRIG        | GPIO 23 (Pin 16) | Pemicu                          |
| ECHO        | Pin 2 (5V)       | Penerima (via pembagi tegangan) | 

### Hasil Setup
![WhatsApp Image 2025-10-15 at 11 27 50_1f974eb5](https://github.com/user-attachments/assets/c0bcb432-8184-4614-a016-ec2fdcdc9cb9)

### Kode Python Untuk Mengukur Jarak
- Masuk ke terminal Raspberry Pi, lalu ketik perintah berikut:
```
nano sensor_jarak.py
```
- Lalu masukkan kode berikut:
```
import RPi.GPIO as GPIO
import time

# Gunakan penomoran BCM
GPIO.setmode(GPIO.BCM)

# Atur GPIO
TRIG = 23
ECHO = 24

GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)

def hitung_jarak():
    # Set TRIG rendah untuk memastikan tidak ada pulsa
    GPIO.output(TRIG, False)
    time.sleep(0.5)

    # Kirim pulsa selama 10 mikrodetik
    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)

    # Waktu sinyal mulai
    while GPIO.input(ECHO) == 0:
        waktu_mulai = time.time()

    # Waktu sinyal diterima kembali
    while GPIO.input(ECHO) == 1:
        waktu_akhir = time.time()

    durasi = waktu_akhir - waktu_mulai

    # Kecepatan suara = 34300 cm/s
    jarak = durasi * 17150
    jarak = round(jarak, 2)

    return jarak

try:
    while True:
        jarak = hitung_jarak()
        print(f"Jarak: {jarak} cm")
        time.sleep(1)

except KeyboardInterrupt:
    print("Menghentikan program")
    GPIO.cleanup()
```

### Penjelasan Kode

```python
import RPi.GPIO as GPIO
import time
# Mengimpor library RPi.GPIO untuk mengontrol pin GPIO pada Raspberry Pi,
# dan library time untuk mengatur jeda dan mencatat waktu.

GPIO.setmode(GPIO.BCM)
# Mengatur mode penomoran pin menggunakan sistem BCM (Broadcom).

TRIG = 23
ECHO = 24
# Menetapkan pin TRIG pada GPIO 23 sebagai pemicu sinyal,
# dan pin ECHO pada GPIO 24 sebagai penerima pantulan sinyal.

GPIO.setup(TRIG, GPIO.OUT)
GPIO.setup(ECHO, GPIO.IN)
# Mengatur TRIG sebagai output dan ECHO sebagai input.

def hitung_jarak():
    GPIO.output(TRIG, False)
    time.sleep(0.5)
    # Memastikan TRIG dalam kondisi rendah agar tidak ada sinyal yang dikirim, lalu menunggu 0.5 detik.

    GPIO.output(TRIG, True)
    time.sleep(0.00001)
    GPIO.output(TRIG, False)
    # Mengirimkan pulsa ultrasonik selama 10 mikrodetik.

    while GPIO.input(ECHO) == 0:
        waktu_mulai = time.time()
    # Menunggu hingga sinyal pantulan diterima dan mencatat waktu mulai.

    while GPIO.input(ECHO) == 1:
        waktu_akhir = time.time()
    # Mencatat waktu saat sinyal pantulan diterima kembali.

    durasi = waktu_akhir - waktu_mulai
    # Menghitung selisih waktu antara pengiriman dan penerimaan sinyal.

    jarak = durasi * 17150
    jarak = round(jarak, 2)
    # Menghitung jarak dengan rumus (durasi × 17150), di mana 17150 adalah
    # kecepatan suara (34300 cm/s) dibagi dua karena sinyal pergi-pulang.
    # Hasil dibulatkan hingga dua angka desimal.

    return jarak

try:
    while True:
        jarak = hitung_jarak()
        print(f"Jarak: {jarak} cm")
        time.sleep(1)
    # Program utama: memanggil fungsi hitung_jarak() terus-menerus
    # dan menampilkan hasil pengukuran setiap 1 detik.

except KeyboardInterrupt:
    print("Menghentikan program")
    GPIO.cleanup()
    # Jika pengguna menekan Ctrl + C, program dihentikan dengan aman
    # dan semua konfigurasi GPIO dikembalikan ke kondisi awal.
```

- Simpan dan jalankan program dengan `Python3`
```
python3 sensor_jarak.py
```

### Hasil
Program akan mencetak hasil dari deteksi sensor jarak setiap 1 detik
![WhatsApp Image 2025-10-15 at 11 28 42_d148b69e](https://github.com/user-attachments/assets/aac7c942-c64c-4f16-94bd-8942d2f7aab5)
