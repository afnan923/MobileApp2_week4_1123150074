# 📋 Ringkasan & Troubleshooting
---

# 🔄 Urutan Request Lengkap (Happy Path)

| No | Endpoint | Expected Response |
|---|---|---|
| 1 | POST `/accounts:signUp` | 200 + `idToken` (emailVerified: false di token) |
| 2 | POST `/accounts:sendOobCode` | 200 + konfirmasi email terkirim |
|   | **User klik link di email** | `emailVerified` berubah menjadi **true** di Firebase |
| 3A | POST `/accounts:lookup` | 200 + `users[0].emailVerified: true` |
| 3B | POST `/auth/verify-token` | 403 jika belum verify, 200 + backend token jika sudah |
| 4 | POST `/accounts:signInWithPassword` | 200 + **idToken baru** (sudah verified) |
| 5 | POST `/auth/verify-token` | 200 + `access_token` (Backend JWT) |
| 6 | GET `/products` | 200 + array produk (Backend API) |

---

# 🛠️ Troubleshooting Common Issues

| Problem | Kemungkinan Penyebab | Solusi |
|---|---|---|
| Step 1 return `EMAIL_EXISTS` | Email sudah pernah didaftarkan | Hapus user di Firebase Console → **Authentication → Users**, lalu register ulang |
| Step 2 return `INVALID_ID_TOKEN` | Token sudah expire (>1 jam) | Login ulang di **Step 4** untuk mendapatkan token baru |
| Step 3 `emailVerified` masih `false` | User belum klik link verifikasi | Buka inbox (cek juga folder **Spam**), lalu klik link verifikasi |
| Step 3B return `403 EMAIL_NOT_VERIFIED` | Email memang belum verified | Klik link verifikasi di email terlebih dahulu |
| Step 5 return `401 TOKEN_EXPIRED` | idToken dari Step 1 sudah kadaluarsa | Lakukan **Step 4 (Login)** untuk mendapatkan token baru |
| Step 6 return `401 MISSING_TOKEN` | Lupa menambahkan header Authorization | Tambahkan header `Authorization: Bearer {{BACKEND_TOKEN}}` |
| Step 6 return `401 INVALID_TOKEN` | Menggunakan Firebase token bukan backend token | Gunakan token dari **Step 5**, bukan dari Step 1 atau 4 |

---

# 📚 Referensi: Firebase Identity Toolkit Endpoints

| Aksi | Endpoint | Method |
|---|---|---|
| Register | `identitytoolkit.googleapis.com/v1/accounts:signUp` | POST |
| Login (Email/Password) | `identitytoolkit.googleapis.com/v1/accounts:signInWithPassword` | POST |
| Login (OAuth Google) | `identitytoolkit.googleapis.com/v1/accounts:signInWithIdp` | POST |
| Kirim Email Verifikasi | `identitytoolkit.googleapis.com/v1/accounts:sendOobCode` | POST |
| Cek Info User | `identitytoolkit.googleapis.com/v1/accounts:lookup` | POST |
| Reset Password | `identitytoolkit.googleapis.com/v1/accounts:sendOobCode` | POST |
| Refresh Token | `securetoken.googleapis.com/v1/token` | POST |
| Hapus Akun | `identitytoolkit.googleapis.com/v1/accounts:delete` | POST |

---

# 🚀 Tahap Selanjutnya

Setelah semua step pengujian di Postman berhasil:

- Implementasikan autentikasi di **Backend**  
- Integrasikan login di aplikasi **Flutter**  
_Postman = Blueprint. Flutter = Implementasi._
