# ⚙️ Step-by-Step Postman

Setelah memahami alur pada **README.md** dan berhasil membuat project di **Firebase Authentication**, sekarang kita akan melanjutkan ke tahap **testing menggunakan Postman**.

Di bagian ini kita akan melakukan beberapa langkah untuk menguji proses autentikasi Firebase melalui API.

---

## 1️⃣ Setup Environment di Postman

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

