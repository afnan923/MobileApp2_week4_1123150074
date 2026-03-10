# 🚀 Firebase Auth + Backend with Postman

Sistem ini menggunakan **Flutter** sebagai frontend, **Firebase** sebagai penyedia layanan autentikasi, serta **Backend API** (Go/PHP/Python) dengan database **MySQL**.

<img width="500" src="https://github.com/user-attachments/assets/d268b941-4a69-415c-993a-99db677fa255" />

---

## 1️⃣ Layer 1: User & Mobile Apps (Frontend)

Layer ini adalah titik awal interaksi pengguna.

1. **Register**  
   User mengisi data di aplikasi Flutter dan mengirim **permintaan pendaftaran**.

2. **Token JWT**  
   Setelah registrasi awal berhasil di Firebase, aplikasi menerima **JWT token** sebagai identitas sementara.

3. **Send Email Verification**  
   Aplikasi memicu pengiriman **email verifikasi** ke pengguna.

---

## 2️⃣ Layer 2: Firebase & Email Service

Layer ini menangani **autentikasi, keamanan, dan pengiriman email**.

- **Firebase Authentication**  
  Menerima permintaan registrasi dan mengelola **status pengguna**.

- **Gmail / SMTP Service**  
  Firebase menggunakan protokol SMTP (Gmail atau layanan lain) untuk mengirim **Email Confirmation** ke inbox pengguna.

- **Verification Process**  
  1. User membuka email yang diterima.  
  2. Klik **link verifikasi**.  
  3. Token verifikasi dikirim kembali ke Firebase.  
  4. Status akun diubah menjadi **Verified**.

---

## 3️⃣ Layer 3: Backend API

Setelah email diverifikasi, Flutter mengirim data ke **Backend API**.

- **Verify Token**  
  Backend memvalidasi token dengan Firebase untuk memastikan:  
  - Token valid  
  - Email user sudah diverifikasi  

- **Security Bridge**  
  Layer ini berfungsi sebagai **gerbang keamanan** sebelum data masuk ke database utama.

---

## 4️⃣ Layer 4: Database (MySQL)

Layer terakhir untuk **penyimpanan permanen data user**.

- **Save User**  
  Setelah backend memastikan user valid:  
  - Nama  
  - Email  
  - ID unik  
  akan disimpan ke **MySQL Database**.

---

## ✅ Ringkasan Alur Kerja

1. **User mendaftar di Apps**  
   Pengguna mengisi formulir pendaftaran di aplikasi Flutter.

2. **Apps mendaftarkan akun ke Firebase**  
   Data user dikirim ke Firebase untuk autentikasi awal.

3. **Firebase mengirim email verifikasi ke User**  
   Firebase mengirim **Email Confirmation** ke inbox pengguna.

4. **User klik link verifikasi di email**  
   Pengguna membuka email dan klik link untuk memverifikasi akun.

5. **Firebase memperbarui status user menjadi "Verified"**  
   Status akun user di Firebase berubah menjadi **Verified**.

6. **Apps mengirim data ke Backend API dengan membawa token**  
   Setelah verifikasi, aplikasi mengirim data user beserta **JWT / Verification Token** ke Backend.

7. **Backend validasi token ke Firebase, lalu simpan data ke MySQL**  
   Backend memeriksa token ke Firebase. Jika valid, data user disimpan secara permanen di database MySQL.

---

## 📁 Project Structure

```
├── README.md                # Arsitektur & alur sistem     
├── firebase/                # Tutorial Settings Firebase       
│   └── firebase.md              
├── postman/                 # Step Testing API
│   ├── step-step.md
│   └── ringkasan.md
└──────────────────────────────────────────────────
```
- [README.md](https://github.com/afnan923/MobileApp2_week4_1123150074/blob/development/README.md) - Firebase Auth + Backend with Postman
- [firebase.md](https://github.com/afnan923/MobileApp2_week4_1123150074/blob/development/firebase/firebase.md) - Firebase Project Setup & Web API Key
- [step-step.md](https://github.com/afnan923/MobileApp2_week4_1123150074/blob/development/postman/step-step.md) - Step-by-Step Postman
- [ringkasan.md](https://github.com/afnan923/MobileApp2_week4_1123150074/blob/development/postman/ringkasan.md) - Ringkasan & Troubleshooting Postman

## Acknowledgments

- [Flutter Community](https://flutter.dev/community) - Amazing frontend framework 
- [Firebase](https://firebase.google.com/) - Auth & email verification services
- [Postman](https://www.postman.com/) - Backend testing and debugging
