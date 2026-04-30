# Project 1: Dockerized Fullstack Application (Node.js & MongoDB)

Tugas ini merupakan implementasi kontainerisasi aplikasi fullstack menggunakan Docker dan Docker Compose, dilengkapi dengan kustomisasi infrastruktur dan eksposur publik menggunakan Ngrok.

## Struktur Proyek

Proyek ini terdiri dari dua layanan utama:

- **App Service**: Aplikasi Node.js dengan framework Express dan EJS sebagai view engine.
- **Database Service**: MongoDB sebagai penyimpanan data postingan.

## Langkah-langkah yang Telah Dilakukan

### 1. Persiapan dan Kloning Repositori

Melakukan kloning repositori dasar dan memeriksa struktur file untuk memahami dependensi antar service.

```bash
git clone [https://github.com/HardevKhandhar/dockerized-fullstack-application](https://github.com/HardevKhandhar/dockerized-fullstack-application)
cd dockerized-fullstack-application
```

### 2. Terapkan Kustomisasi Wajib & Tantangan Tambahan

Melakukan modifikasi pada file docker-compose.yml untuk memenuhi kriteria tugas dan tantangan teknis:

- Port Mapping: Mengubah port host dari 3000 menjadi 8080 untuk menghindari konflik port.
- Volume Persistence: Menambahkan volume mongo_data pada service MongoDB agar data tidak hilang saat container dihapus.
- Environment Variables: Memasukkan variabel lingkungan (DB_HOST, DB_PORT, MONGO_INITDB_DATABASE) untuk konfigurasi infrastruktur yang lebih profesional.

### 3. Eksekusi Docker Compose

Membangun image dan menjalankan container di latar belakang.

```Bash
docker-compose up -d --build
```

Verifikasi status container menggunakan:

```Bash
docker ps
```

### 4. Konfigurasi Ngrok (Public Tunneling)

Menggunakan Ngrok untuk membuat tunnel aman dari localhost ke internet publik agar aplikasi dapat diakses oleh dosen/penguji secara remote.

```PowerShell
.\\ngrok config add-authtoken <YOUR_AUTHTOKEN>
.\\ngrok http 8080
```

## Detail Kustomisasi (docker-compose.yml)

Berikut adalah perubahan signifikan yang dilakukan pada konfigurasi:

```YAML
services:
  app:
    ports:
      - "8080:3000" # Host:Container
    environment:
      - DB_HOST=mongo
      - DB_PORT=27017

  mongo:
    environment:
      - MONGO_INITDB_DATABASE=app
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:
```

## Cara Verifikasi

Akses Lokal: Buka http://localhost:8080 di browser.

Akses Publik: Gunakan URL Forwarding dari Ngrok (misal: https://saloon-empirical-wriggly.ngrok-free.dev).

Inspeksi Traffic: Buka Dashboard Ngrok di http://127.0.0.1:4040.
