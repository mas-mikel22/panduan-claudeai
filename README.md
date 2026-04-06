# 📘 Panduan Instalasi Claude Code CLI

**Dibuat oleh [DigitalHub Indonesia](https://github.com/mas-mikel22)**

Panduan lengkap untuk menginstall dan menggunakan Claude Code CLI untuk chat interface langsung di terminal VS Code.

---

## 💰 Metode Pembayaran

Untuk dukungan dan konsultasi, Anda dapat memberikan donasi melalui:

| Metode | Nomor |
|--------|-------|
| **ShopeePay** | `082 322 515 580` |
| **OVO** | `082 322 515 580` |

Terima kasih atas dukungan Anda!

---

## 📑 Daftar Isi

- [Apa itu Claude Code CLI?](#-apa-itu-claude-code-cli)
- [Panduan Instalasi](#-panduan-instalasi)
  - [🪟 Windows](#-windows)
  - [🍎 Mac/Linux](#-maclinux)
- [Cara Menggunakan](#-cara-menggunakan)
- [FAQ - Pertanyaan Umum](#-faq---pertanyaan-umum)
- [Troubleshooting](#-troubleshooting)
- [Keamanan](#️-keamanan)
- [Referensi](#-referensi-dan-file-terkait)

---

## 🎯 Apa itu Claude Code CLI?

**Claude Code** adalah tool dari Anthropic yang memungkinkan Anda untuk chat dengan Claude AI. Ada **2 cara** menggunakan Claude Code:

### 🔧 Opsi 1: Claude Code CLI (Terminal)
- ✅ Chat dengan Claude langsung di **terminal** (PowerShell, CMD, Bash)
- ✅ Menggunakan **command line interface**
- ✅ Perlu setup **environment variables** (`setx` di Windows)
- ✅ Jalankan dengan command `claude` di terminal

### 🎨 Opsi 2: VS Code Extension (Panel UI)
- ✅ Chat dengan Claude di **panel sidebar VS Code**
- ✅ Menggunakan **graphical user interface**
- ✅ Bisa setup dengan **Environment Variables** (sama seperti CLI) atau **VS Code settings.json**
- ✅ Akses melalui ikon Claude di sidebar VS Code

### 📊 Perbandingan

| Aspek | CLI (Terminal) | VS Code Extension |
|-------|----------------|-------------------|
| **Interface** | Terminal/Command Line | Panel UI di VS Code |
| **Cara Akses** | Jalankan `claude` di terminal | Klik ikon Claude di sidebar |
| **Konfigurasi** | Environment variables (`setx`) | Environment variables (`setx`) atau VS Code settings.json |
| **File Config** | `~/.claude/settings.json` | VS Code User/Workspace Settings |
| **Cocok Untuk** | Developer yang suka terminal | Developer yang suka UI visual |

**💡 Pilih salah satu atau gunakan keduanya!** Anda bisa setup keduanya untuk fleksibilitas maksimal.

---

## 🚀 Panduan Instalasi

### Prerequisites

- **Node.js 18 atau lebih baru** terinstall di sistem Anda
  - Download: [nodejs.org](https://nodejs.org)

### 🪟 Windows

#### Step 1: Install Claude Code CLI

```powershell
npm install -g @anthropic-ai/claude-code
```

**Catatan:** Jika muncul error permission, jalankan PowerShell sebagai Administrator.

#### Step 2: Install Extension Claude Code for VS Code

**Jika belum terinstall**, install extension **Claude Code** di VS Code:

1. **Buka VS Code Extensions:**
   - klik ikon **Extensions** di sidebar kiri

2. **Cari "Claude Code":**
   - Di search box, ketik: `Claude Code`
   - Pilih extension **"Claude Code"** oleh Anthropic
   - Klik **Install**

3. **Tunggu hingga instalasi selesai**

#### Step 3: Setup Konfigurasi

**Pilih salah satu atau setup keduanya:**

---

##### **A. Setup untuk CLI (Terminal)** 🔧

Jika Anda ingin menggunakan Claude Code di **terminal**, setup environment variables:

**Windows (CMD atau PowerShell):**

Ganti `your_api_key` dengan API key Anda yang sebenarnya, lalu jalankan:

```cmd
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.minimax.io/anthropic
```

**Catatan Penting:**
- `setx` akan set environment variables secara **permanen** di sistem Windows
- Setelah menjalankan `setx`, Anda **harus tutup terminal dan buka terminal baru** agar environment variables aktif, kalau di terminal VS code tutup VS code dan buka kembali
- Ganti `your_api_key` dengan API key Anda yang sebenarnya (tanpa tanda kutip)

> [!IMPORTANT]
> **Setelah setup `setx` selesai:**
> - Saat pertama kali menjalankan `claude`, Anda hanya perlu **memilih tema** (Dark mode/Light mode)
> - Claude Code akan **otomatis menggunakan API**
> - **TIDAK PERLU** membuka https://console.anthropic.com/ atau setup API key manual
> - Jika diminta API key, berarti environment variables belum terbaca - restart terminal dan coba lagi

**Alternatif: Buat File settings.json untuk CLI**

Atau buat file `~/.claude/settings.json` untuk konfigurasi CLI:

**Step 1: Buat Folder .claude**

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"
```

**Step 2: Buat File settings.json**

Copy-paste perintah ini **SEKALIGUS** ke PowerShell (ganti `your_api_key` dengan API key Anda):

```powershell
@"
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_api_key",
    "ANTHROPIC_BASE_URL": "https://api.minimax.io/anthropic",
    "API_TIMEOUT_MS": "3000000"
  }
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding utf8
```

**Step 3: Verifikasi File Sudah Dibuat**

```powershell
# Cek apakah file ada
Test-Path "$env:USERPROFILE\.claude\settings.json"

# Lihat isi file
Get-Content "$env:USERPROFILE\.claude\settings.json"
```

**Step 4: Edit File (Jika Perlu)**

```powershell
# Buka dengan Notepad
notepad "$env:USERPROFILE\.claude\settings.json"

# Atau buka dengan VS Code
code "$env:USERPROFILE\.claude\settings.json"
```

📄 **Untuk troubleshooting CLI**, lihat: [`TROUBLESHOOTING.md` - Claude Code Dialihkan ke Anthropic Console](TROUBLESHOOTING.md#claude-code-dialihkan-ke-anthropic-console-login)
---

##### **B. Setup untuk VS Code Extension (Panel UI)** 🎨

> [!TIP]
> **Kabar Baik!** VS Code Extension juga bisa menggunakan **System Environment Variables** sama seperti CLI.
> Jadi jika sudah setup CLI dengan `setx`, VS Code Extension **otomatis bisa langsung digunakan** tanpa konfigurasi tambahan!

> [!IMPORTANT]
> Jika Anda sudah setup environment variables untuk CLI di langkah sebelumnya, **Anda tidak perlu melakukan apa-apa lagi!**
> Langsung lanjut ke **Step 4: Mulai Menggunakan**.

**Jika belum setup**, jalankan perintah berikut di **CMD atau PowerShell**:

```cmd
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.minimax.io/anthropic
```

**Langkah selanjutnya:**
1. Ganti `your_api_key` dengan API key Anda yang sebenarnya
2. **Tutup VS Code sepenuhnya** dan buka kembali
3. VS Code Extension akan otomatis membaca environment variables dari sistem
4. **Selesai!** ✅

**Hasil:**
- ✅ Claude Code extension langsung menggunakan API
- ✅ Tidak perlu konfigurasi `settings.json`
- ✅ Berlaku untuk semua project (global)
- ✅ **Satu setup untuk CLI dan Extension!**

📄 **Untuk troubleshooting Extension**, lihat: [`TROUBLESHOOTING.md` - Setup Claude Code Extension untuk VS Code](TROUBLESHOOTING.md#setup-claude-code-extension-untuk-vs-code)


#### Step 4: Mulai Menggunakan

**Opsi 1: Menggunakan VS Code Extension (Direkomendasikan)**

**⚠️ PENTING:** Setelah konfigurasi, **jangan** menjalankan `claude` di terminal. Gunakan VS Code Extension panel di sidebar.

**Cara membuka VS Code Extension panel:**

1. **Pastikan VS Code sudah di-reload** setelah konfigurasi settings.json
   - Tutup dan buka VS Code lagi

2. **Buka Claude Code Extension panel:**
   - Klik ikon **Claude Code** (ikon orange dengan starburst) di **sidebar kiri** VS Code

3. **Panel Claude Code akan muncul di sidebar** (bukan di terminal!)
   - Panel akan menampilkan chat interface dengan Claude
   - Bisa langsung mulai chat tanpa perlu menjalankan command di terminal

4. **Mulai chat** dengan Claude langsung di panel VS Code! 🎉

**Catatan:**
- ✅ VS Code Extension panel = Interface chat di sidebar VS Code (seperti screenshot extension)
- ❌ CLI di terminal = Interface chat di terminal (yang muncul saat menjalankan `claude` command)
- Jika Anda melihat interface di terminal, berarti Anda menggunakan CLI, bukan Extension panel

**Opsi 2: Menggunakan CLI di Terminal**

1. **Tutup terminal saat ini dan buka terminal baru** (penting! agar environment variables aktif)

2. **Navigasi ke folder project:**
   ```powershell
   cd your-project-directory
   ```

3. **Jalankan Claude Code:**
   ```powershell
   claude
   ```

4. **Selesai!** Anda sekarang bisa chat dengan Claude Code di terminal! 🎉

---

### 🍎 Mac/Linux

#### Step 1: Install Claude Code CLI

```bash
npm install -g @anthropic-ai/claude-code
```

**Catatan:** Jika muncul permission error, gunakan `sudo`:
```bash
sudo npm install -g @anthropic-ai/claude-code
```

#### Step 2: Install Extension Claude Code for VS Code

**Jika belum terinstall**, install extension **Claude Code** di VS Code:

1. **Buka VS Code Extensions:**
   - Tekan `Ctrl+Shift+X` (Linux) atau `Cmd+Shift+X` (Mac)
   - Atau klik ikon **Extensions** di sidebar kiri

2. **Cari "Claude Code":**
   - Di search box, ketik: `Claude Code`
   - Pilih extension **"Claude Code"** oleh Anthropic
   - Klik **Install**

3. **Tunggu hingga instalasi selesai**

#### Step 3: Setup Konfigurasi

> [!NOTE]
> Pilih salah satu cara berikut. **Automated Script** adalah cara termudah!

---

##### **Cara 1: Automated Script (Direkomendasikan) ⭐**

Jalankan perintah berikut di terminal:

```bash
curl -O "https://cdn.bigmodel.cn/install/claude_code_zai_env.sh" && bash claude_code_zai_env.sh
```

Script akan otomatis mengubah file `~/.claude/settings.json` dengan konfigurasi berikut:

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
    "ANTHROPIC_BASE_URL": "https://api.minimax.io/anthropic",
    "API_TIMEOUT_MS": "3000000"
  }
}
```

**Catatan:** Ganti `your_zai_api_key` dengan API key Anda yang sebenarnya saat diminta oleh script.

---

##### **Cara 2: Manual Configuration**

Jika Anda sudah pernah mengkonfigurasi environment variables untuk Claude Code, Anda bisa mengkonfigurasi secara manual. **Restart terminal diperlukan** agar perubahan berlaku.

**Edit file `~/.claude/settings.json`:**

```bash
# Buat folder jika belum ada
mkdir -p ~/.claude

# Edit file settings.json
nano ~/.claude/settings.json
```

**Tambahkan/ubah konfigurasi berikut:**

```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
    "ANTHROPIC_BASE_URL": "https://api.minimax.io/anthropic",
    "API_TIMEOUT_MS": "3000000"
  }
}
```

**Catatan:** Ganti `your_zai_api_key` dengan API key Anda yang sebenarnya.

**Langkah selanjutnya:**
1. Simpan file (`Ctrl+X`, lalu `Y`, lalu `Enter` untuk nano)
2. **Tutup terminal dan buka terminal baru**
3. **Selesai!** ✅

---

**Hasil:**
- ✅ Claude Code CLI langsung menggunakan API
- ✅ Claude Code VS Code Extension juga otomatis bekerja
- ✅ **Satu setup untuk CLI dan Extension!**

📄 **Untuk troubleshooting**, lihat: [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md)


#### Step 4: Mulai Menggunakan

**Opsi 1: Menggunakan VS Code Extension (Direkomendasikan)**

**⚠️ PENTING:** Setelah konfigurasi, **jangan** menjalankan `claude` di terminal. Gunakan VS Code Extension panel di sidebar.

**Cara membuka VS Code Extension panel:**

1. **Pastikan VS Code sudah di-reload** setelah konfigurasi settings.json
   - Tekan `Ctrl+Shift+P` (Linux) atau `Cmd+Shift+P` (Mac)
   - Ketik: `Developer: Reload Window`
   - Atau tutup dan buka VS Code lagi

2. **Buka Claude Code Extension panel:**
   - **Cara 1:** Klik ikon **Claude Code** (ikon orange dengan starburst) di **sidebar kiri** VS Code
   - **Cara 2:** Tekan `Ctrl+Shift+P` (Linux) atau `Cmd+Shift+P` (Mac), ketik `Claude Code: Open` atau `Claude Code: Focus`
   - **Cara 3:** Klik menu **View** → **Open View...** → cari "Claude Code"

3. **Panel Claude Code akan muncul di sidebar** (bukan di terminal!)
   - Panel akan menampilkan chat interface dengan Claude
   - Bisa langsung mulai chat tanpa perlu menjalankan command di terminal

4. **Mulai chat** dengan Claude langsung di panel VS Code! 🎉

**Catatan:**
- ✅ VS Code Extension panel = Interface chat di sidebar VS Code (seperti screenshot extension)
- ❌ CLI di terminal = Interface chat di terminal (yang muncul saat menjalankan `claude` command)
- Jika Anda melihat interface di terminal, berarti Anda menggunakan CLI, bukan Extension panel

**Opsi 2: Menggunakan CLI di Terminal**

```bash
cd your-project-directory
claude
```

---

## 💡 Cara Menggunakan

### Dasar-dasar

Setelah Claude Code berjalan di terminal:

1. **Kirim chat** langsung dengan mengetik pesan Anda
2. **Tekan Enter** untuk mengirim
3. **Tunggu response** dari Claude
4. **Lanjutkan percakapan** dengan chat berikutnya

### Command Khusus

**Command yang tersedia di Claude Code CLI:**

- `/status` - Cek status model yang sedang digunakan
- `/cost` - Cek penggunaan token dan biaya untuk sesi saat ini
- `/help` - Lihat daftar command yang tersedia
- `/exit` - Keluar dari Claude Code
- `/model` - Ganti atau cek model yang digunakan

### 📊 Cara Cek Penggunaan Limit

📄 **Lihat:** [`PENJELASAN_RATE_LIMIT.md`](PENJELASAN_RATE_LIMIT.md) - Penjelasan lengkap tentang limit dan cara cek penggunaan

**Ringkasan:**
- **Paket Lite:** Hingga ~120 perintah setiap 5 jam — sekitar 3× kuota penggunaan paket Claude Pro
- Command `/cost` hanya menampilkan token usage, bukan jumlah prompt
- Tidak ada metode langsung untuk cek jumlah prompt yang digunakan
- Gunakan tracking manual atau monitor error 429 untuk mengetahui limit

**Command untuk cek usage:**
- `/cost` - Cek token usage (input + output tokens)
- `/status` - Cek status model dan konfigurasi

### Tips

- Claude Code akan otomatis membaca konteks dari file di folder project Anda
- Gunakan untuk bertanya tentang kode, minta bantuan coding, atau diskusi tentang project

---

## ❓ FAQ - Pertanyaan Umum

### Apakah saya perlu install Claude Code CLI?

**Ya**, jika Anda ingin:
- Chat interface langsung di terminal VS Code
- Interaksi real-time dengan Claude sambil coding
- Context-aware terhadap file di project

Lihat bagian [Panduan Instalasi](#-panduan-instalasi) di atas untuk panduan lengkap.

### Bagaimana workflow penggunaannya?

1. **Setup sekali** (setelah dapat API key):
   - Install Claude Code CLI: `npm install -g @anthropic-ai/claude-code`
   - Setup konfigurasi dengan `setx` atau script otomatis

2. **Gunakan di terminal:**
   - Buka terminal baru (setelah setup)
   - Navigasi ke folder project: `cd your-project`
   - Jalankan: `claude`
   - Mulai chat!

### Apakah setup perlu diulang setiap kali?

**Tidak!** Setelah setup sekali:
- Environment variables sudah tersimpan permanen
- Cukup buka terminal baru dan jalankan `claude`
- Tidak perlu setup ulang

### Apakah bisa ganti model yang digunakan?

Ya, gunakan command `/model` di dalam Claude Code CLI untuk melihat dan mengganti model yang digunakan.

---

## 🔍 Troubleshooting

Untuk solusi lengkap berbagai error yang mungkin terjadi, lihat:

📄 **[`TROUBLESHOOTING.md`](TROUBLESHOOTING.md)** - Panduan troubleshooting lengkap

File tersebut mencakup:
- ✅ Error instalasi (npm not recognized, execution policy, network error)
- ✅ Error konfigurasi (script tidak bisa dijalankan, konfigurasi tidak bekerja)
- ✅ Error runtime (git-bash required, akses file, command not found)
- ✅ Solusi lengkap untuk setiap error dengan penjelasan detail


---

## ⚠️ Keamanan

1. **JANGAN** commit API key ke Git atau version control
2. **JANGAN** share API key dengan orang lain
3. **JANGAN** screenshot terminal yang menampilkan API key
4. **GUNAKAN** environment variables (sudah di-setup otomatis)
5. **REVIEW** permission yang diberikan saat Claude Code meminta akses folder

---

## 📚 Referensi dan File Terkait

### 📄 File di Repository Ini:
- [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) - Panduan troubleshooting lengkap untuk semua error
- [`PENJELASAN_RATE_LIMIT.md`](PENJELASAN_RATE_LIMIT.md) - Penjelasan tentang rate limit

---

## 🆘 Bantuan

Jika masih mengalami masalah:

1. Cek panduan di bagian [Panduan Instalasi](#-panduan-instalasi) di atas
2. Pastikan API key Anda valid dan belum expired
3. Pastikan BASE URL benar: `https://api.minimax.io/anthropic`
4. Cek [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) untuk solusi lengkap
5. Pastikan Node.js versi 18+ sudah terinstall

---

**Selamat menggunakan Claude Code! 🎉**

---

## 👤 Tentang DigitalHub Indonesia

**DigitalHub Indonesia** adalah penyedia solusi digital yang berfokus pada pengembangan teknologi AI, otomatisasi, dan konsultasi IT di Indonesia.

Kami menyediakan:
- Panduan dan tutorial teknologi terbaru
- Solusi integrasi API dan AI
- Konsultasi pengembangan perangkat lunak
- Workshop dan training teknologi

**Hubungi kami:**
- GitHub: [@mas-mikel22](https://github.com/mas-mikel22)
- Repository: [panduan-claudeai](https://github.com/mas-mikel22/panduan-claudeai)

---
# panduan-claudeai
