# ⚙️ Step-by-Step Postman

Setelah memahami alur pada **README.md** dan berhasil membuat project di **Firebase Authentication**, sekarang kita akan melanjutkan ke tahap **testing menggunakan Postman**.

Di bagian ini kita akan melakukan beberapa langkah untuk menguji proses autentikasi Firebase melalui API.

---

## Setup Environment di Postman

1. Buka **Postman**.
2. Pada menu sebelah kiri klik **Environments**.
<img width="600"  src="https://github.com/user-attachments/assets/a5cd8452-322b-48a7-8643-cd2111e04f64" />


3. Klik icon **+** di kiri atas untuk membuat environment baru.
4. Beri nama environment, misalnya:
```
Firebase Auth Dev
```
5. Tambahkan variabel berikut di environment:

| Variable           | Initial Value            | Keterangan                                   |
|-------------------|-------------------------|---------------------------------------------|
| FIREBASE_API_KEY    | AIzaSyB_xxx...          | Web API Key dari Firebase Console           |
| FIREBASE_ID_TOKEN   | *(kosong)*              | Diisi otomatis setelah login (via Test script) |
| BACKEND_BASE_URL    | http://localhost:8080/v1 | Base URL backend kamu                        |
| BACKEND_TOKEN       | *(kosong)*              | Token JWT dari backend (diisi setelah verify) |
| USER_EMAIL          | test@example.com        | Email untuk testing                          |
| USER_PASSWORD       | Test@12345              | Password untuk testing                       |

> **Catatan:** Untuk variabel yang bertuliskan `*(kosong)*`, biarkan kosong dulu karena nanti akan otomatis terisi setelah login atau verify token.
<img width="600"  src="https://github.com/user-attachments/assets/f6b77782-6bd6-4ad7-ac32-e06a890a083d" />

💡 **Tips:**  
- Gunakan format `{{VARIABLE_NAME}}` di request Postman, misalnya `{{FIREBASE_API_KEY}}`.  
- Dengan environment ini, semua request bisa langsung menggunakan variabel tanpa harus copy-paste manual setiap kali.  

---

## 1️⃣ Step Register / Membuat Akun Baru
### Endpoint
POST
```
 https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
```
### Headers

| Key          | Value            | Keterangan                                  |
| ------------ | ---------------- | ------------------------------------------- |
| Content-Type | application/json | Wajib untuk semua request Firebase REST API |

### Request Body (raw JSON) 
```
{
  "email": "{{USER_EMAIL}}",
  "password": "{{USER_PASSWORD}}",
  "returnSecureToken": true
}
```
| Field             | Tipe    | Keterangan                                              |
| ----------------- | ------- | ------------------------------------------------------- |
| email             | string  | Email yang akan didaftarkan                             |
| password          | string  | Password minimal 6 karakter                             |
| returnSecureToken | boolean | Harus bernilai `true` agar Firebase mengembalikan token |

### Response Sukses : 200 OK 
```
{
 "localId": "UID_USER",
 "email": "test@example.com",
 "idToken": "FIREBASE_ID_TOKEN",
 "refreshToken": "REFRESH_TOKEN",
 "expiresIn": "3600"
}
```
<img width="700"  src="https://github.com/user-attachments/assets/bca8687f-1587-477c-b60e-1fefecef9abb" />
<img width="700"  src="https://github.com/user-attachments/assets/923106d6-0d0c-4115-b176-d38741ec09fb" />
<img width="700"  src="https://github.com/user-attachments/assets/0b50b18f-683f-440d-af35-e9910c936c2a" />

### Response Error : 400 Bad Request
```
{ 
    "error": { 
        "code": 400, 
        "message": "EMAIL_EXISTS",       // ← Email sudah terdaftar 
        "status": "INVALID_ARGUMENT" 
    } 
} 
```
<img width="700"  src="https://github.com/user-attachments/assets/05cd2e43-f2a8-4e10-a392-3ecbf20a3f69" />

### Error Code

| Error Code                  | Artinya                         | Solusi                              |
| --------------------------- | ------------------------------- | ----------------------------------- |
| EMAIL_EXISTS                | Email sudah terdaftar           | Gunakan email lain                  |
| INVALID_EMAIL               | Format email tidak valid        | Gunakan format email yang benar     |
| WEAK_PASSWORD               | Password kurang dari 6 karakter | Gunakan password minimal 6 karakter |
| OPERATION_NOT_ALLOWED       | Email/password auth belum aktif | Aktifkan di Firebase Console        |
| TOO_MANY_ATTEMPTS_TRY_LATER | Terlalu banyak percobaan        | Tunggu beberapa menit               |

---
### Postman Test Script — Auto-save Token
Copy paste ke tab "Tests" di Postman agar idToken tersimpan otomatis: 
```
// Postman → Tests tab: 
const json = pm.response.json(); 
 
if (pm.response.code === 200) { 
    pm.environment.set("FIREBASE_ID_TOKEN", json.idToken); 
    pm.environment.set("FIREBASE_LOCAL_ID", json.localId); 
    pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken); 
    console.log("Register sukses. UID:", json.localId); 
    console.log("PERHATIAN: Email belum diverifikasi. Lanjut ke Step 2."); 
} else { 
    console.log("Register gagal:", json.error.message); 
}
```
<img width="700" src="https://github.com/user-attachments/assets/6eaf8861-3c1e-4631-a976-920464e0aefb" />

##  2️⃣ Kirim Email Verification
### Endpoint
POST
```
 https://identitytoolkit.googleapis.com/v1/accounts:sendOobCode?key={{FIREBASE_API_KEY}}
```
### Headers

| Key          | Value            | Keterangan                                  |
| ------------ | ---------------- | ------------------------------------------- |
| Content-Type | application/json | Wajib untuk semua request Firebase REST API |

### Request Body (raw JSON) 
```
{
 "requestType": "VERIFY_EMAIL",
 "idToken": "{{FIREBASE_ID_TOKEN}}"
}
```
| Field       | Tipe   | Keterangan                  |
| ----------- | ------ | --------------------------- |
| requestType | string | Harus `VERIFY_EMAIL`        |
| idToken     | string | Firebase ID Token dari user |


### Response Sukses : 200 OK 
```
{ 
    "kind": "identitytoolkit#GetOobConfirmationCodeResponse", 
    "email": "test@example.com"    // ← Email tujuan pengiriman 
} 
```
<img width="700" src="https://github.com/user-attachments/assets/1b674e28-b0a2-450e-bc23-c3bb5876aec1" />
<img width="700" src="https://github.com/user-attachments/assets/ca5a6265-d9df-420a-b024-82963efa3d1c" />


### Response Error : 400 Bad Request
```
{ 
    "error": { 
        "code": 400, 
        "message": "EMAIL_EXISTS",       // ← Email sudah terdaftar 
        "status": "INVALID_ARGUMENT" 
    } 
} 
```

### Error Code

| Error Code                  | Artinya                             | Solusi                     |
| --------------------------- | ----------------------------------- | -------------------------- |
| INVALID_ID_TOKEN            | Token tidak valid atau expired      | Login ulang                |
| USER_NOT_FOUND              | User tidak ditemukan                | Pastikan register berhasil |
| TOO_MANY_ATTEMPTS_TRY_LATER | Firebase membatasi pengiriman email | Tunggu beberapa menit      |

---

### Email yang di terima verify email 

<img width="700" src="https://github.com/user-attachments/assets/5cceeb07-71b1-46b6-8430-e2a7a3631d56" />

### Setelah confirmation 

<img width="700" src="https://github.com/user-attachments/assets/c57ebedf-aec7-459a-bc74-b6ed608be02c" />

----

### Postman Test Script — Auto-save Token
Copy paste ke tab "Tests" di Postman agar idToken tersimpan otomatis: 
```
// Postman → Tests tab: 
if (pm.response.code === 200) { 
const json = pm.response.json(); 
console.log("Email verifikasi dikirim ke:", json.email); 
console.log("Sekarang buka inbox email dan klik link verifikasi."); 
console.log("Setelah klik, lanjut ke Step 3 untuk cek status."); 
} else { 
console.log("Gagal kirim email:", pm.response.json().error.message); 
} 
```

##  3️⃣ Cek Status Verifikasi Email dengan CARA A — Cek langsung ke Firebase (accounts:lookup)
### Endpoint

POST
```
 https://identitytoolkit.googleapis.com/v1/accounts:lookup?key={{FIREBASE_API_KEY}}
```

### Request Body (raw JSON) 
```
{ 
    "idToken": "{{FIREBASE_ID_TOKEN}}" 
} 
```
<img width="1377" height="502" alt="Screenshot 2026-03-10 161838" src="https://github.com/user-attachments/assets/59dd8867-a3f9-4c23-8119-69f1f6768ea2" />


### Response — Email BELUM diverifikasi 
```
{ 
    "kind": "identitytoolkit#GetAccountInfoResponse", 
    "users": [ 
        { 
            "localId": "aBcDeFgHiJkLmN", 
            "email": "test@example.com", 
            "displayName": "Test User", 
            "passwordHash": "UkVEQUNURUQ=", 
            "emailVerified": false,          // ← BELUM DIVERIFIKASI 
            "passwordUpdatedAt": 1700000000000, 
            "providerUserInfo": [ ... ], 
            "validSince": "1700000000", 
            "lastLoginAt": "1700000000000", 
            "createdAt": "1700000000000" 
        } 
    ] 
} 
```


### Response — Email SUDAH diverifikasi
```
{ 
    "kind": "identitytoolkit#GetAccountInfoResponse", 
    "users": [ 
        { 
            "localId": "aBcDeFgHiJkLmN", 
            "email": "test@example.com", 
            "emailVerified": true,           // ← SUDAH DIVERIFIKASI 
            "lastLoginAt": "1700000000000", 
            ... 
        } 
    ] 
}
```
<img width="1296" height="547" alt="Screenshot 2026-03-10 161848" src="https://github.com/user-attachments/assets/9f2ae14d-004f-4ca6-b392-c10f49adca5b" />

## 4️⃣ Login dengan Email & Password 
```
Mengapa perlu Login setelah sudah Register? 
idToken dari Step 1 (Register) sudah kadaluarsa setelah 1 jam. 
Setelah user klik link verifikasi di email → status berubah, tapi idToken lama tidak otomatis 
terupdate. 
Dengan Login ulang → Firebase mengembalikan idToken BARU yang sudah merefleksikan 
emailVerified: true.
```
### Endpoint
```
POST https://identitytoolkit.googleapis.com/v1/accounts:signInWithPassword?key={{FIREBASE_API_KEY}}
```

### Request Body (raw JSON) 
```
{ 
    "email": "{{USER_EMAIL}}", 
    "password": "{{USER_PASSWORD}}", 
    "returnSecureToken": true 
}
```

### Response Sukses : 200 OK 
```
{ 
    "kind": "identitytoolkit#VerifyPasswordResponse", 
    "localId": "aBcDeFgHiJkLmN", 
    "email": "test@example.com", 
    "displayName": "Test User", 
    "idToken": "eyJhbGciOiJSUzI1...",    // ← Firebase ID Token BARU 
    "registered": true, 
    "refreshToken": "AMf-vBxK...", 
    "expiresIn": "3600" 
} 
```
<img width="1336" height="741" alt="Screenshot 2026-03-10 162522" src="https://github.com/user-attachments/assets/13df086b-b3fc-46a1-becb-e081de8027c9" />

----

### Decode Firebase ID Token untuk melihat isinya 
```
Salin idToken dan paste ke: jwt.io 
Di bagian Payload, cari field "email_verified". 
Jika user sudah klik link verifikasi: "email_verified": true 
Jika belum: "email_verified": false 
Ini membuktikan bahwa informasi email_verified ada DALAM token itu sendiri.
```
### Contoh Decoded Firebase JWT Payload (di jwt.io)
```
{ 
    "iss": "https://securetoken.google.com/YOUR_PROJECT_ID", 
    "aud": "YOUR_PROJECT_ID", 
    "auth_time": 1700000000, 
    "user_id": "aBcDeFgHiJkLmN", 
    "sub": "aBcDeFgHiJkLmN", 
    "iat": 1700000000, 
    "exp": 1700003600, 
    "email": "test@example.com", 
    "email_verified": true,         // ← FIELD INI yang dibaca backend 
    "firebase": { 
        "identities": { 
            "email": ["test@example.com"] 
        }, 
        "sign_in_provider": "password" 
    } 
}
```
<img width="824" height="538" alt="jwt" src="https://github.com/user-attachments/assets/dcae6d3b-7da3-4f59-a85a-1707e565761d" />
### Response Error : 400 Bad Request
```
{ 
    "error": { 
        "code": 400, 
        "message": "INVALID_PASSWORD", 
        "errors": [{ "message": "INVALID_PASSWORD", "domain": "global" }] 
    } 
}  
```

### Error Code

| Error Code                  | Artinya                                              | Solusi                                       |
| --------------------------- | ---------------------------------------------------- | -------------------------------------------- |
| INVALID_PASSWORD            | Password salah                                       | Cek kembali password yang dimasukkan         |
| EMAIL_NOT_FOUND             | Email belum terdaftar                                | Lakukan register terlebih dahulu pada Step 1 |
| USER_DISABLED               | Akun dinonaktifkan oleh admin                        | Hubungi admin melalui Firebase Console       |
| INVALID_EMAIL               | Format email tidak valid                             | Pastikan format email benar                  |
| TOO_MANY_ATTEMPTS_TRY_LATER | Login diblokir karena terlalu banyak percobaan gagal | Tunggu beberapa menit                        |

---
### Postman Test Script — Auto-save Token
Copy paste ke tab "Tests" di Postman agar idToken tersimpan otomatis: 
```
// Postman → Tests tab: 
const json = pm.response.json(); 

if (pm.response.code === 200) { 
// Update environment dengan idToken BARU hasil login 
pm.environment.set("FIREBASE_ID_TOKEN", json.idToken); 
pm.environment.set("FIREBASE_REFRESH_TOKEN", json.refreshToken); 
console.log("Login berhasil. Token diperbarui."); 
console.log("Lanjut ke Step 5: kirim token ke backend."); 
} else { 
console.log("Login gagal:", json.error.message); 
}
```

## 5️⃣ POST Firebase Token ke Backend

Kita kirim Firebase ID Token (dari Login) ke endpoint backend /auth/verify-token.  
Backend decode token, verifikasi ke Google, cek emailVerified.  
Backend membuat user baru di database (jika first time), atau update last login.  
Backend return JWT token miliknya sendiri — INILAH yang dipakai untuk semua request selanjutnya.  

### Endpoint

POST 
```{{BACKEND_BASE_URL}}/auth/verify-token```

### Headers

| Key | Value | Keterangan |
|---|---|---|
| Content-Type | application/json | Wajib untuk mengirim JSON |
| Accept | application/json | Opsional namun disarankan |

### Request Body

```json
{
  "firebase_token": "{{FIREBASE_ID_TOKEN}}"
}
```
### Field / Key

| Field          | Tipe   | Keterangan                                       |
| -------------- | ------ | ------------------------------------------------ |
| firebase_token | string | Firebase ID Token yang didapat dari proses login |

### Response Sukses
```
{
  "success": true,
  "data": {
    "access_token": "BACKEND_JWT_TOKEN",
    "token_type": "Bearer",
    "expires_in": 86400,
    "user": {
      "uid": "aBcDeFgHiJkLmN",
      "email": "test@example.com",
      "email_verified": true
    }
  }
}
```
### Error Code
| Error Code             | Artinya                    | Solusi                                    |
| ---------------------- | -------------------------- | ----------------------------------------- |
| EMAIL_NOT_VERIFIED     | Email belum diverifikasi   | Buka inbox email dan klik link verifikasi |
| INVALID_FIREBASE_TOKEN | Firebase token tidak valid | Login ulang untuk mendapatkan token baru  |
| TOKEN_EXPIRED          | Token sudah kadaluarsa     | Lakukan login ulang                       |
### Postman Test Script
```
const json = pm.response.json();

if (pm.response.code === 200) {
    pm.environment.set("BACKEND_TOKEN", json.data.access_token);
    console.log("Backend token berhasil disimpan.");
}
```




