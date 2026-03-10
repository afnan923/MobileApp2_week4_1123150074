# 🔥 Firebase Project Setup & Web API Key

Tutorial ini menjelaskan cara membuat project di **Firebase** dan menemukan **Web API Key** yang digunakan untuk autentikasi atau integrasi aplikasi (web, mobile, atau backend).

---

## 1. Membuat Project Firebase

1. Buka website Firebase Console
   https://console.firebase.google.com
    <img width="600"  src="https://github.com/user-attachments/assets/f0fdc606-45dc-4c98-936b-53d45fcc15af" />


3. Klik **Create a Project** atau **Add Project**.

4. Masukkan **nama project** yang diinginkan.
   Contoh:

   ```
   postman
   ```
    <img width="600"  src="https://github.com/user-attachments/assets/d7a599c4-ab5c-48c4-a914-f9de43136429" />

5. Klik **Continue**.

6. Pilih pengaturan **Google Analytics**:

   * Bisa **Enable** atau **Disable**.
    <img width="600"  src="https://github.com/user-attachments/assets/a1c7b9e4-d17f-4632-9011-9bf112e7e8c8" />

7. Klik **Create Project**.

    <img width="600"  src="https://github.com/user-attachments/assets/7bf682dc-0019-43d8-98f8-85196544a05d" />

9. Tunggu proses pembuatan project selesai lalu klik **Continue**.

---

## 2. Menambahkan Menu Authentication

1. Masuk ke Firebase Console.

2. Pilih project yang sudah dibuat.

3. Pada menu sebelah kiri, klik **Build**.

<img width="600" src="https://github.com/user-attachments/assets/1383a991-fb25-410d-9d5e-6973d194c288" />

4. Pilih **Authentication** lalu klik **Get Started** untuk mengaktifkan layanan autentikasi.

<img width="600" src="https://github.com/user-attachments/assets/2ff21b29-92af-40fb-8f54-11f1ccce8fc0" />

5. Setelah Authentication aktif, buka tab **Sign-in method**.

6. Pilih provider **Email/Password**.

7. Aktifkan opsi **Enable** pada Email/Password Authentication.

8. Klik **Save** untuk menyimpan pengaturan.

9. Tampilan setelah **Authentication Email/Password berhasil diaktifkan**.

<img width="600" src="https://github.com/user-attachments/assets/0184d89a-227f-487e-aff8-2895c85d15e0" />

> ⚠️ Pastikan **Email/Password provider sudah aktif**, karena metode ini akan digunakan untuk proses **register dan login melalui API Firebase**.
---

## 3. Menambahkan Aplikasi Web

Setelah project berhasil dibuat:

1. Di halaman **Project Overview**, klik ikon **Web (</>)**.
  <img width="600"  src="https://github.com/user-attachments/assets/40aa2b1e-fb51-40b4-85f8-a4e5b55abee0" />

  <img width="600"  src="https://github.com/user-attachments/assets/9e6a679f-ebda-45e5-880d-759b881904f2" />

2. Masukkan **App nickname**.
   Contoh:

   ```
   my web
   ```
   <img width="600"  src="https://github.com/user-attachments/assets/be4a5db8-71c2-4497-b7be-b16f1014cfd1" />

3. Klik **Register App**.
4. Jika sudah klik **Register App** lalu klik **Continue** saja.
  <img width="600"  src="https://github.com/user-attachments/assets/39b2d2b1-2afd-4224-9900-8450c504f3bf" />
---

## 3. Menemukan Web API Key

Setelah aplikasi web terdaftar, Firebase akan menampilkan **konfigurasi SDK** seperti berikut:

```
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXX",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdefg"
};
```
  <img width="600"  src="https://github.com/user-attachments/assets/534484b0-63b2-4b19-82f7-7a67749454b5" />

Bagian yang digunakan sebagai **Web API Key** adalah:

```
apiKey
```

Contoh:

```
apiKey: "AIzaSyXXXXXXXXXXXX"
```

API Key ini digunakan untuk:

* Autentikasi Firebase
* Integrasi dengan aplikasi frontend
* Testing API menggunakan Postman

---

## 4. Alternatif Cara Melihat API Key

API Key juga bisa ditemukan melalui pengaturan project:

1. Masuk ke **Firebase Console**.
2. Klik **Project Settings**.
3. Pilih tab **General**.
4. Scroll ke bagian **Your Apps**.
5. Pilih aplikasi web yang sudah dibuat.
6. Lihat bagian **SDK setup and configuration**.

Di sana akan terlihat **firebaseConfig** yang berisi **apiKey**.

---

## 5. Catatan Penting

* Web API Key **bukan secret key**, sehingga aman digunakan di aplikasi frontend.
* Namun tetap **jangan menyalahgunakan API Key**.
* Gunakan **Firebase Security Rules** untuk melindungi database dan layanan lainnya.

---

✍️ Tutorial ini digunakan untuk membantu proses integrasi Firebase Authentication dengan aplikasi frontend, backend API, atau pengujian menggunakan Postman.
