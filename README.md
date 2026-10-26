# **FRES-Master-Alarm: Penjelasan Pembaruan Sistem Komunikasi**

Berikut adalah penjelasan mengenai pembaruan terbaru pada sistem FRES-Master-Alarm, yang dirancang untuk meningkatkan keandalan komunikasi antar perangkat.

----------

### **Pembaruan Fitur: Metode Sinkronisasi Baru via Sinyal Wi-Fi**

Pada versi terbaru, memperkenalkan metode sinkronisasi yang lebih andal antara **Perangkat Induk (Master)** dan **Perangkat Anak (Slave)**.

Sebelumnya, Perangkat Induk menyiarkan informasi perubahan **jalur komunikasi (channel)** ke semua Perangkat Anak. Metode ini memiliki kelemahan: jika Perangkat Induk sudah terhubung ke internet dan berganti jalur komunikasi, siaran informasinya seringkali tidak diterima oleh Perangkat Anak karena mereka masih "mendengarkan" di jalur yang lama. Hal ini menyebabkan kegagalan sinkronisasi.

Solusi Terbaru:

Kini, Perangkat Master akan memancarkan sinyal Wi-Fi pribadinya (Access Point). Sinyal ini berfungsi sebagai penanda bagi Perangkat Slave untuk secara otomatis mengetahui jalur komunikasi yang benar. Dengan cara ini:

-   **Komunikasi Lebih Andal:** Perangkat Slave hanya perlu terhubung ke sinyal Wi-Fi khusus ini untuk langsung menyamakan jalur komunikasinya, menghilangkan risiko kegagalan sinkronisasi.
    
-   **Sinyal Tersembunyi dan Aman:** Untuk keamanan dan kerapian jaringan, sinyal Wi-Fi yang dipancarkan oleh Perangkat Induk akan disembunyikan (_hidden_) dan hanya dapat mendukung hingga 7 Perangkat Anak.
    
-   **Identitas Unik:** Setiap Perangkat Induk akan memiliki nama sinyal Wi-Fi (SSID) yang unik, ditandai dengan nomor seri produk pada akhir namanya (contoh: `MASTER-ALARM-AB12`).
    

----------

### **Cara Kerja Sistem**

Berikut adalah alur kerja sistem dari awal instalasi hingga beroperasi normal:

1.  **Inisiasi Perangkat Induk:** Saat pertama kali dinyalakan, Perangkat Master akan aktif dalam mode ganda: siap terhubung ke internet (seperti Wi-Fi rumah Sakit) sekaligus mampu memancarkan sinyalnya sendiri.

2.  **Komunikasi Awal:** Semua perangkat diatur untuk berkomunikasi pada jalur standar (default channel 6).
    
3.  **Koneksi Internet & Penyesuaian:** Ketika Perangkat Master berhasil terhubung ke jaringan internet, sistem akan memeriksa apakah channel komunikasinya berubah. Jika ya, Perangkat Master akan memulai ulang (restart) secara otomatis untuk menerapkan pengaturan channel  baru.
    
4.  **Pemancaran Sinyal Pribadi:** Setelah terhubung ke internet, Perangkat Master mulai memancarkan sinyal Wi-Fi pribadinya. Sinyal inilah yang akan menjadi panduan bagi Perangkat Slave.
    
5.  **Sinkronisasi Perangkat Slave:** Perangkat Slave secara otomatis akan mencari dan terhubung ke sinyal Wi-Fi milik Perangkat Master. Setelah terhubung, jalur komunikasinya akan langsung disesuaikan. Jika sinyal tidak ditemukan, Perangkat Slave akan tetap beroperasi pada channel standar.
    
6.  **Stabilitas Sistem:** Untuk memastikan semua perubahan berjalan dengan sempurna, Perangkat Master dirancang untuk selalu memulai ulang sistemnya setiap kali terjadi pergantian jalur komunikasi.

----------
### **Cara Konfigurasi Sistem ESPnow Master**
1. **Inisialisasi Sandi :** Menginputkan Sandi untuk mengakses halaman pengaturan. 
2. **Inisialisasi Serial Number:** Masukkan Serial Number sesuai dengan sistem produksi.
3. **Tampilan Central:** Masukkan jenis central menyesuaikan kebutuhan produksi.

----------
### **Cara Konfigurasi Sistem ESPnow Slave**
1. **Inisialisasi Device ID:** Input nilai sesuai dengan urutan tampilan pada tampilan central di master.
2. **Inisialisasi Master SN:** Masukkan Serial Number Master yang terhubung dalam satu ekosistem.
3. **Master Addres:** Masukkan mac address yang tampil pada halaman pengaturan Master Alarm.


### Program Master Alarm Touchscreen FRES

Saat pertama clone, periksa dan pastikan konfigurasi library berjalan dengan benar.
Tambahkan konfigurasi di bawah ini pada Library LedControl dan Nextion.

**Nextion**
==============

**NexConfig.h**

    #define dbSerial Serial
    #define nexSerial Serial2

**NexHardware.cpp**

    dbSerialBegin(115200);
    nexSerial.begin(115200);
    
    ret1 = recvRetCommandFinished(100);
    ret2 = recvRetCommandFinished(100);

**Python3**
==============
Buat Environtment terisolasi Python: (Test pada MacOS 12.7.6)
 
    python3 -m venv myenv
    source myenv/bin/activate
    python3 -m pip install scons


Buil project untuk mengambil firmware.bin

    pio run -t clean
    pio run