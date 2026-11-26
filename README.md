# 🤖 Alter Dark Bot (`.adb`) — Dokumentasi Format File Konfigurasi

![Format](https://img.shields.io/badge/Format-AQW--BotManager-blueviolet?style=for-the-badge)
![Version](https://img.shields.io/badge/Schema%20Version-v2-blue?style=for-the-badge)
![Encoding](https://img.shields.io/badge/Encoding-JSON%20(UTF--8)-success?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Android-informational?style=for-the-badge)

**File `.adb` (AlterDark Bot)** adalah file konfigurasi skrip bot yang di-generate dan dibaca langsung oleh **Bot Manager** pada **Alter Dark Bot** untuk game *AdventureQuest Worlds (AQW)*.

Meskipun berekstensi `.adb`, isi file ini sebenarnya menggunakan format **JSON (UTF-8)** yang terstruktur rapi (indentasi 2 spasi), sehingga mudah dibaca, di-backup, dibagikan ke komunitas, maupun dimodifikasi secara manual menggunakan text editor biasa.

---

## 📑 Daftar Isi

- [✨ Fitur Utama Format `.adb`](#-fitur-utama-format-adb)
- [📂 Lokasi Penyimpanan & Penamaan Default](#-lokasi-penyimpanan--penamaan-default)
- [🧬 Struktur JSON File `.adb`](#-struktur-json-file-adb)
- [📖 Penjelasan Detail Properti](#-penjelasan-detail-properti)
  - [1. Header & Metadata](#1-header--metadata)
  - [2. `cmdData` (Daftar Command)](#2-cmddata-daftar-command)
  - [3. `skillData` (Rotasi Skill)](#3-skilldata-rotasi-skill)
  - [4. `questData` (Daftar Quest)](#4-questdata-daftar-quest)
  - [5. `dropData` (Whitelist / Auto-Pick Drop)](#5-dropdata-whitelist--auto-pick-drop)
  - [6. `settings` (Pengaturan UI & Opsi Bot)](#6-settings-pengaturan-ui--opsi-bot)
- [🔄 Cara Save & Load di Bot Manager](#-cara-save--load-di-bot-manager)
- [🔌 Kompatibilitas dengan `.gbot`](#-kompatibilitas-dengan-gbot)
- [💡 Tips Edit Manual](#-tips-edit-manual)

---

## ✨ Fitur Utama Format `.adb`

- **Human-Readable JSON**: Disimpan menggunakan `JSON.stringify(..., null, 2)` sehingga rapi dan mudah diinspeksi.
- **All-in-One Config**: Menyimpan urutan command, rotasi skill, daftar quest, daftar item drop, hingga pengaturan terakhir di UI Bot Manager dalam satu file.
- **Cross-Platform**: Dapat digunakan baik di **Windows (PC)** maupun **Android (Mobile)**.
- **Auto-Sanitized**: Saat proses *Save* maupun *Load*, Bot Manager otomatis membersihkan field sementara (seperti status internal `hasAttemptedJoin`) agar konfigurasi tetap bersih dan siap dijalankan ulang.

---

## 📂 Lokasi Penyimpanan & Penamaan Default

### 🏷️ Penamaan File Default (PC)
Saat Anda menekan tombol **Save** di Bot Manager tanpa mengetikkan nama file secara manual:
- Jika karakter sedang berada di sebuah map: **`Bot_<NamaMap>.adb`** (contoh: `Bot_battleon.adb`)
- Jika nama map tidak terdeteksi: **`BotConfig.adb`**

> **Catatan:** Jika Anda menyimpan tanpa menuliskan ekstensi, Bot Manager akan otomatis menambahkan akhiran `.adb`.

### 📁 Direktori Penyimpanan
| Platform | Lokasi Penyimpanan / Mekanisme |
| :--- | :--- |
| **Windows (PC)** | Dipilih bebas melalui dialog **File Explorer** (`FileReference`), atau melalui folder custom yang diatur di menu **Custom Bot Directory**. |
| **Android (Mobile)** | Disimpan secara otomatis ke direktori `/storage/emulated/0/ADB` (dengan fallback ke `<UserDirectory>/ADB` atau `<DocumentsDirectory>/ADB` jika folder utama tidak tersedia). |

---

## 🧬 Struktur JSON File `.adb`

Berikut adalah contoh struktur lengkap dari sebuah file `.adb` versi `2`:

```json
{
  "format": "AQW-BotManager",
  "version": 2,
  "savedAt": "Thu Nov 26 15:30:00 GMT+0700 2026",
  "cmdData": [
    {
      "rawLabel": "Join: battleon-1e99",
      "label": "Join: battleon-1e99",
      "type": "joinmap",
      "data": {
        "map": "battleon",
        "cell": "Enter",
        "pad": "Spawn"
      }
    }
  ],
  "skillData": [
    {
      "rawLabel": "1: Skill 1",
      "label": "1: Skill 1",
      "slot": 1
    },
    {
      "rawLabel": "2: Skill 2",
      "label": "2: Skill 2",
      "slot": 2
    }
  ],
  "questData": [
    {
      "rawLabel": "Quest: 1234",
      "label": "Quest: 1234",
      "id": "1234"
    }
  ],
  "dropData": [
    {
      "rawLabel": "Voucher of Nulgath",
      "label": "Voucher of Nulgath",
      "type": "getdrop",
      "data": {
        "name": "Voucher of Nulgath",
        "id": "4929",
        "itemId": "4929"
      }
    }
  ],
  "settings": {
    "text": {
      "txt_Room": "1e99",
      "txt_Cell": "Enter",
      "txt_Pad": "Spawn",
      "txt_HpRest": "40",
      "txt_MpRest": "40"
    },
    "num": {
      "num_Skill": 1,
      "num_MiscDelay": 1000,
      "num_Index": 0
    },
    "anonymous": false,
    "privateRoom": true,
    "itemsTemp": false,
    "soundItems": []
  }
}
```

---

## 📖 Penjelasan Detail Properti

### 1. Header & Metadata

| Key | Tipe | Keterangan |
| :--- | :--- | :--- |
| `format` | `String` | Penanda format resmi file, bernilai `"AQW-BotManager"`. |
| `version` | `Number` | Versi skema konfigurasi saat ini, bernilai `2`. |
| `savedAt` | `String` | Timestamp waktu file dibuat/disimpan. |
| `Author` / `author` *(opsional)* | `String` | Nama pembuat bot. Jika ada di file, otomatis ditampilkan ke kolom Author saat di-load. |
| `Description` / `description` *(opsional)* | `String` | Deskripsi singkat fungsi bot. Otomatis ditampilkan ke kolom Description saat di-load. |

---

### 2. `cmdData` (Daftar Command)

Berisi array urutan langkah (*step-by-step*) yang akan dieksekusi oleh bot dari atas ke bawah.

| Field | Tipe | Keterangan |
| :--- | :--- | :--- |
| `rawLabel` | `String` | Teks label asli command sebelum diberi nomor urut indeks di list UI. |
| `label` | `String` | Teks tampilan command pada daftar Bot Manager. |
| `type` | `String` | Jenis aksi command (misalnya `joinmap`, `kill`, `rest`, `getdrop`, `delay`, `label`, `gotolabel`, dll.). |
| `data` | `Object` | Objek parameter spesifik yang dibutuhkan oleh `type` command tersebut. |

---

### 3. `skillData` (Rotasi Skill)

Berisi daftar urutan skill yang akan digunakan secara berulang saat bot melakukan combat.

| Field | Tipe | Keterangan |
| :--- | :--- | :--- |
| `rawLabel` | `String` | Label asli skill (misal `"1: Skill 1"`). |
| `label` | `String` | Teks tampilan skill pada list UI. |
| `slot` | `Number` | Nomor slot skill yang valid, bernilai **`1` hingga `6`**. |

---

### 4. `questData` (Daftar Quest)

Berisi daftar Quest yang akan dipantau, diterima (*accept*), atau diselesaikan (*turn-in*) secara otomatis oleh bot.

| Field | Tipe | Keterangan |
| :--- | :--- | :--- |
| `rawLabel` | `String` | Format default `"Quest: <ID>"`. |
| `label` | `String` | Teks tampilan quest di UI. |
| `id` | `String` | ID numerik Quest AQW (harus berupa angka valid). |

---

### 5. `dropData` (Whitelist / Auto-Pick Drop)

Berisi daftar item drop monster yang ingin diambil otomatis oleh bot.

| Field | Tipe | Keterangan |
| :--- | :--- | :--- |
| `rawLabel` | `String` | Nama item, atau `"Drop ID: <id>"` jika hanya ID yang diisi. |
| `label` | `String` | Teks tampilan item di tab Drop. |
| `type` | `String` | Selalu bernilai `"getdrop"`. |
| `data.name` | `String` | Nama item drop. |
| `data.id` | `String` | Item ID (jika diketahui). |
| `data.itemId` | `String` | Salinan Item ID untuk kompatibilitas internal. |

---

### 6. `settings` (Pengaturan UI & Opsi Bot)

Menyimpan kondisi input dan checkbox pada Bot Manager agar saat file di-load kembali, seluruh pengaturan langsung terisi seperti semula.

- **`text`**: Menyimpan isi kolom teks seperti `txt_Room`, `txt_Cell`, `txt_Pad`, `txt_HpRest`, `txt_MpRest`, `txt_CoorX`, `txt_CoorY`, `txt_GotoPlayer`, `txt_QuestID`, `txt_SpamPacket`, `txt_Monster`, `txt_MonsterWithItem`, `txt_ItemMonsterDrop`, `txt_ItemDropQty`, `txt_GetDrop`, `txt_GetDropCmd`, `txt_ItemID`, `txt_MapItem`, `txt_MiscInput`, `txt_Label`, `txt_GotoLabel`, `txt_BankItem`, dan `txt_SoundItemName`.
- **`num`**: Menyimpan nilai angka pada stepper (`num_Skill`, `num_MiscDelay`, `num_Index`).
- **`anonymous`** (`Boolean`): Status mode anonymous.
- **`privateRoom`** (`Boolean`): Status opsi masuk ke private room.
- **`itemsTemp`** (`Boolean`): Status opsi radio *Temporary Items*.
- **`soundItems`** (`Array`): Daftar item yang memicu notifikasi suara (`{ "label": "...", "data": "..." }`).

---

## 🔄 Cara Save & Load di Bot Manager

### 💾 Menyimpan Konfigurasi (Save)
1. Susun **Command**, **Skill**, **Quest**, **Drop**, dan **Settings** sesuai kebutuhan farming Anda di Bot Manager.
2. Klik tombol **Save**:
   - **Di Windows**: Pilih lokasi folder, masukkan nama file (misalnya `Farm_LR.adb`), lalu klik Save.
   - **Di Android**: Masukkan nama file pada modal **Mobile Bot Storage**, lalu simpan (otomatis masuk ke `/storage/emulated/0/ADB`).

### 📥 Memuat Konfigurasi (Load)
1. Klik tombol **Load** (atau pilih langsung dari daftar **Custom Bot Directory** / **Mobile Bot Storage**).
2. Pilih file `.adb` yang diinginkan.
3. Bot Manager akan mem-parsing JSON, mengisi seluruh tab (`Command`, `Skill`, `Quest`, `Drop`), menerapkan `settings`, lalu menampilkan notifikasi jumlah item yang berhasil dimuat.

---

## 🔌 Kompatibilitas dengan `.gbot`

Selain format native **`.adb`**, Bot Manager Alter Dark Bot juga mendukung pembacaan file **`.gbot` (Grimoire Bot)**. Saat Anda memuat file `.gbot` yang memiliki struktur `Commands.$values`, Bot Manager akan otomatis mengonversinya melalui parser internal (`parseGbotFormat`) ke dalam struktur data Bot Manager sehingga dapat langsung dijalankan maupun disimpan ulang sebagai file **`.adb`**.

---

## 💡 Tips Edit Manual

- Gunakan editor seperti **VS Code**, **Notepad++**, atau **Sublime Text** dengan mode bahasa **JSON** agar syntax error (seperti koma berlebih atau tanda kutip yang hilang) langsung terlihat.
- Pastikan file tetap disimpan dengan encoding **UTF-8** (tanpa atau dengan BOM, keduanya didukung oleh parser Bot Manager).
- Jika ingin membagikan script ke orang lain, Anda dapat menambahkan properti `"Author"` dan `"Description"` di level paling atas JSON agar kredit dan panduan singkat muncul otomatis saat file di-load.
