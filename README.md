# Backup Mysql Database Dump
## Auto Dump Database Setiap Hari

### Fitur
- Melakukan dump database MySQL remote (jarak jauh) langsung ke dalam direktori backups.
- Mengirimkan notifikasi ke saluran (channel) Discord Anda melalui webhook.
- Menghapus cadangan (backup) lama secara otomatis setelah melewati jumlah hari tertentu untuk menghemat ruang penyimpanan pada disk lokal Anda.

### System requirements
- Node.js versi 16 atau yang lebih tinggi.
- Versi modifikasi dari mysqldump di dalam direktori node_modules agar dapat berfungsi dengan Node.js v16 ke atas.
- Direkomendasikan: Menggunakan PM2 untuk menjalankan prosesnya.

**Note:** Alat ini membutuhkan komputer lokal Anda dalam keadaan menyala, proses aplikasi sedang berjalan, dan koneksi internet yang stabil agar dapat berfungsi. Jika proses pencadangan gagal pada waktu yang telah ditentukan, proses tidak akan diulang kembali hingga jadwal di hari berikutnya.

### Installation
```
git clone https://github.com/Gembuls/BackUP-DB.git
npm install
```

CUbah nilai-nilai di dalam file config_sample.json sesuai dengan data Anda, lalu ubah nama file tersebut menjadi `config.json`.
```js
{
    "backup_filename_prefix": "bot_name_here",
    "mysql":
        {
            "host": "db.host.tld",
            "user": "db_user",
            "password": "db_pass",
            "database": "db_name"
        },
    "discord": 
        { 
            "webhook": "https://discord.com/api/webhooks/CHANNEL_ID/WEBHOOK",
            "username": "Webhook Name",
            "owner_id": "bot_owner_discord_id"
        },
    "days_to_keep": 1
}
```
Anda perlu membuat webhook Discord terlebih dahulu di salah satu channel Anda, kemudian tempelkan (paste) tautan tersebut ke dalam file konfigurasi di atas.

`owner_id` adalah ID Discord pengguna yang akan di-ping oleh webhook saat notifikasi dikirimkan. Anda juga bisa membuat webhook ini melakukan ping ke suatu role tertentu dengan menambahkan awalan `&` sebelum ID role, contohnya: `&761916863847333928`.

Atur jumlah hari untuk menyimpan file cadangan lama melalui nilai days_to_keep pada file `config.json` Anda.

### Startup

1. Buka jendela terminal baru di dalam direktori proyek Anda.
2. Jalankan perintah: pm2 startup index.js --name mysql_backup --time.
3. Anda akan menerima konfirmasi bahwa aplikasi telah berjalan dan semua aktivitas dapat dipantau melalui log. Jalankan pm2 logs mysql_backup untuk melihat log secara real-time.