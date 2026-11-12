# EL BUSINESS HUB — MVP

Aplikasi MVP untuk empat modul:
1. Business Marketplace
2. Property Marketplace
3. Supplier Directory
4. Business Services

## Fitur yang tersedia
- Registrasi dan login dengan password hash.
- Role pengguna dan administrator.
- Pengiriman listing untuk empat modul.
- Moderasi listing oleh admin (pending / approved / rejected).
- Katalog publik dan pencarian.
- Formulir inquiry dari calon pembeli.
- Dashboard pengguna dan dashboard admin.
- Health check di `/health`.
- SQLite untuk uji lokal; dapat diarahkan ke PostgreSQL melalui `DATABASE_URL`.

## Jalankan di Windows

Buka Command Prompt pada folder proyek:

```bat
py -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
set SECRET_KEY=ganti-dengan-kunci-rahasia-panjang
flask --app app init-db
flask --app app create-admin
flask --app app run
```

Buka `http://127.0.0.1:5000`.

Saat perintah `create-admin` berjalan, masukkan email, nama admin, dan password minimal 12 karakter.

## Jalankan di Linux / PythonAnywhere

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export SECRET_KEY='ganti-dengan-kunci-rahasia-panjang'
flask --app app init-db
flask --app app create-admin
gunicorn app:app
```

Untuk PythonAnywhere, sesuaikan WSGI agar mengimpor `app` dari `app.py`, lalu atur `SECRET_KEY` dan `DATABASE_URL` di konfigurasi environment hosting jika tersedia. Jangan menaruh password atau secret di repository publik.

## Struktur

```text
el-business-hub/
├── app.py
├── requirements.txt
├── Procfile
├── .env.example
├── README.md
├── templates/
│   ├── base.html
│   ├── index.html
│   ├── auth.html
│   ├── dashboard.html
│   ├── listing_form.html
│   ├── detail.html
│   └── admin.html
└── static/
    └── style.css
```

## Catatan sebelum produksi
Ini adalah MVP fungsional untuk alur listing dan inquiry, bukan platform pembayaran produksi. Sebelum dibuka untuk transaksi publik:
- Ganti `SECRET_KEY` dengan nilai acak yang kuat.
- Gunakan PostgreSQL dengan backup otomatis.
- Tambahkan CSRF protection, rate limiting, email verification, reset password, dan logging.
- Terapkan HTTPS dan kebijakan keamanan header.
- Tambahkan RLS bila database dipindahkan ke Supabase.
- Integrasikan payment gateway melalui backend dan webhook terverifikasi sebelum menjual paket otomatis.
- Upload foto belum diaktifkan pada MVP ini; modul media perlu ditambahkan dengan validasi tipe/ukuran file dan penyimpanan privat/publik yang tepat.
- Tambahkan pagination, moderasi spam, kebijakan privasi, syarat layanan, dan prosedur penghapusan data.

## Endpoint pemeriksaan
`GET /health` mengembalikan status aplikasi dan koneksi database.
