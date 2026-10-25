# 🤖 elaina-base

Base bot Telegram yang ringan, rapi, dan gampang dikembangkan.
Dibuat dengan **Node.js** dan **node-telegram-bot-api**, dan dirancang supaya
pemula pun bisa langsung menjalankan dan menambah fitur sendiri.

![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18-339933?logo=node.js&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-Bot_API-26A5E4?logo=telegram&logoColor=white)

---

## ✨ Fitur Bawaan

| Perintah  | Fungsi                                                   | Akses        |
|-----------|----------------------------------------------------------|--------------|
| `/start`  | Menu utama beserta info bot dan tombol channel/group     | Semua orang  |
| `/ping`   | Cek latensi, status, uptime, dan pemakaian RAM           | Semua orang  |
| `/tourl`  | Reply foto, video, audio, atau file → dapat link langsung | Semua orang  |
| `/backup` | Membuat `bot-backup.zip` dari seluruh source dan mengirimnya | Owner saja   |

Semua fitur tersimpan di folder `menu/`, jadi menambah atau menghapus fitur
cukup dengan menambah atau menghapus file.

---

## 📋 Kebutuhan

| Kebutuhan | Keterangan |
|-----------|------------|
| **Node.js 18+** | Download di [nodejs.org](https://nodejs.org). Cek versi dengan `node -v` |
| **Token bot** | Buat lewat [@BotFather](https://t.me/BotFather) dengan perintah `/newbot` |
| **ID Telegram** | Cek lewat [@userinfobot](https://t.me/userinfobot) |
| **zip** *(opsional)* | Dibutuhkan hanya untuk `/backup` |

Instal `zip` jika perlu:
- Ubuntu / Debian: `sudo apt install zip`
- Termux: `pkg install zip`

---

## 🚀 Instalasi

**1. Ambil project**

```bash
git clone https://github.com/Lixxx101/elaina-base.git
cd elaina-base
```

Atau klik **Code → Download ZIP** di GitHub lalu extract.

**2. Masuk ke folder project**

```bash
cd elaina-base
```

**3. Install dependensi**

```bash
npm install
```

**4. Isi konfigurasi**

Buka `config.js` dan ganti dua nilai utama berikut:

```js
botToken: "ISI_TOKEN_BOT_KAMU",
ownerId: "ISI_ID_TELEGRAM_KAMU",
```

Contoh format (pakai nilai milikmu sendiri):

```js
botToken: "123456789:AAExampleTokenDariBotFather",
ownerId: "987654321",
```

**5. Jalankan bot**

```bash
npm start
```

Jika berhasil, terminal menampilkan **BOT AKTIF**. Buka bot kamu di Telegram
lalu kirim `/start`.

> Hentikan bot dengan `Ctrl + C`.

---

## ⚙️ Konfigurasi

Semua pengaturan ada di `config.js`:

| Kunci | Fungsi |
|-------|--------|
| `botToken` | Token bot dari @BotFather |
| `ownerId` | ID Telegram owner. Hanya owner yang bisa memakai `/backup` |
| `namebot` | Nama bot yang tampil di menu `/start` |
| `nameown` | Nama developer yang tampil di menu `/start` |
| `Poto` | URL foto untuk menu `/start` |
| `channelLink`, `channelLink2`, `groupLink` | Link tombol di menu `/start` |

Setelah mengubah `config.js`, restart bot agar perubahan terbaca.

---

## 📁 Struktur Project

```
elaina-base/
├── index.js            # Menjalankan bot dan memuat semua file di menu/
├── config.js           # Pengaturan bot (token, owner, nama, link)
├── package.json        # Daftar dependensi
├── database/           # Data penyimpanan (termasuk folder backup)
└── menu/               # Semua fitur bot
    ├── menu.js         # Perintah /start
    ├── tools-ping.js   # Perintah /ping
    ├── tools-tourl.js  # Perintah /tourl
    └── tools-backup.js # Perintah /backup
```

**Aturan utama:** setiap file `.js` di dalam folder `menu/` otomatis dimuat
saat bot dijalankan. Tidak perlu mendaftarkannya di mana pun.

---

## 🧩 Membuat Fitur Baru

Satu fitur = satu file di folder `menu/`. Contoh paling sederhana:

```js
// menu/hello.js
module.exports = (bot) => {
  bot.onText(/^\/hello$/i, (msg) => {
    bot.sendMessage(msg.chat.id, "Halo! 👋");
  });
};
```

Simpan file, restart bot, lalu coba ketik `/hello`.

### Yang perlu dipahami

| Bagian | Artinya |
|--------|---------|
| `module.exports = (bot) => { ... }` | Bentuk wajib setiap file fitur. `bot` diberikan otomatis oleh `index.js` |
| `bot.onText(/regex/, callback)` | Menjalankan `callback` saat pesan cocok dengan regex |
| `msg.chat.id` | ID chat tempat pesan dikirim. Dipakai untuk membalas |
| `msg.from.id` | ID pengirim pesan |
| `bot.sendMessage(chatId, teks)` | Mengirim pesan teks |

### Contoh: perintah khusus owner

```js
// menu/owner-info.js
const config = require("../config");

module.exports = (bot) => {
  bot.onText(/^\/ownerinfo$/i, (msg) => {
    if (String(msg.from.id) !== String(config.ownerId)) {
      return bot.sendMessage(msg.chat.id, "❌ Perintah ini khusus owner.");
    }
    bot.sendMessage(msg.chat.id, "✅ Kamu adalah owner bot.");
  });
};
```

---

## 🛠️ Troubleshooting

| Gejala | Penyebab umum | Solusi |
|--------|---------------|--------|
| `Cannot find module ...` | Dependensi belum terpasang | Jalankan `npm install` |
| Bot tidak merespon | Token salah atau belum diisi | Cek `botToken` di `config.js` |
| `ETELEGRAM: 409 Conflict` | Bot berjalan di dua tempat sekaligus | Matikan semua proses bot lalu jalankan ulang |
| `Gagal load namafile.js` | Ada kesalahan kode di file itu | Periksa file `menu/namafile.js` |
| `/backup` gagal | Perintah `zip` belum terpasang | Instal `zip` (lihat bagian Kebutuhan) |
| `/tourl` gagal | Ukuran file terlalu besar atau koneksi bermasalah | Coba file yang lebih kecil lalu ulangi |

---

## 🔒 Keamanan

- **Jangan pernah** mengunggah `config.js` yang berisi token asli ke GitHub.
- Jika token bocor, buka @BotFather lalu kirim `/revoke` untuk membuat token baru.
- Jangan upload file backup (`.zip`) ke GitHub karena isinya bisa memuat token.

---

## 🙏 Credits

- **Developer:** Lixx
- **Library:** [node-telegram-bot-api](https://github.com/yagop/node-telegram-bot-api), [axios](https://axios-http.com), [chalk](https://github.com/chalk/chalk), [form-data](https://github.com/form-data/form-data)

Kalau kamu memakai atau mengembangkan base ini, tolong cantumkan credit developer di atas.

---

## 📜 Lisensi

Project ini dibagikan untuk belajar dan pengembangan pribadi.
Silakan digunakan dan dimodifikasi dengan tetap mencantumkan credit developer.
