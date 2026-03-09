<img width="1866" height="82" alt="image" src="https://github.com/user-attachments/assets/35151ec9-cd40-459b-90dd-c7e71779b8f4" /># ⚙️ Step-by-Step Postman

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
```
POST https://identitytoolkit.googleapis.com/v1/accounts:signUp?key={{FIREBASE_API_KEY}}
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
