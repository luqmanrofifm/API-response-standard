# 📖 Pedoman Standarisasi Respons API

Dokumen ini berisi panduan dan standarisasi format respons API (Application Programming Interface) untuk memastikan interoperabilitas, konsistensi, dan kemudahan integrasi data antar layanan/sistem.

## 🎯 Tujuan Standarisasi
1. **Konsistensi Data:** Memastikan semua *client* (Web, Mobile, Dasbor) menerima struktur data yang seragam.
2. **Kemudahan Integrasi:** Mengurangi waktu *parsing* data bagi tim *frontend*.
3. **Keamanan & Debugging:** Menyeragamkan format *error* tanpa mengekspos celah sistem (*stack trace*).

---

## 📦 Struktur Dasar (*Envelope*)

Semua respons API **wajib** mengembalikan format JSON dengan struktur dasar (pembungkus) sebagai berikut:

- `code` (Integer): Kode status HTTP.
- `status` (String): `"success"` atau `"error"`.
- `message` (String): Pesan deskriptif mengenai hasil eksekusi.
- `data` (Object/Array/Null): Muatan (*payload*) utama dari respons.
- `errors` (Object/Null): Detail kesalahan (hanya muncul saat gagal/error).
- `meta` (Object/Null): Informasi tambahan seperti paginasi (opsional).

---

## ✅ 1. Respons Berhasil (Success Response)

Digunakan ketika *request* berhasil diproses oleh *server*.

### A. Tanpa Paginasi
```json
{
  "code": 200,
  "status": "success",
  "message": "Data berhasil diambil",
  "data": {
    "id": 1,
    "nama_program": "Program Bantuan",
    "status": "Aktif"
  }
}
```
### B. Pengambilan Data Koleksi (Dengan Paginasi)
```json
{
  "code": 200,
  "status": "success",
  "message": "Daftar program pemerintah berhasil diambil",
  "data": [
    { "id": 1, "nama_program": "Bantuan Sosial Terpadu" },
    { "id": 2, "nama_program": "Pembangunan Infrastruktur Cerdas" }
  ],
  "meta": {
    "current_page": 1,
    "total_page": 12,
    "per_page": 10,
    "total_data": 120
  }
}
```

## ❌ 2. Respons Gagal (error Response)

Digunakan ketika terjadi kesalahan eksekusi. Properti data harus bernilai `null`

### A. Kesalahan Server (Internal Server Error)
```json
{
  "code": 500,
  "status": "error",
  "message": "Terjadi kesalahan internal pada server aplikasi",
  "data": null,
  "errors": null
}
```
### B. Contoh kesalahan validasi
```json
{
  "code": 400,
  "status": "error",
  "message": "Validasi input gagal",
  "data": null,
  "errors": {
    "tanggal": ["Format tanggal tidak valid, gunakan YYYY-MM-DD"],
    "nik": ["NIK harus terdiri dari 16 digit"]
  }
}
```
