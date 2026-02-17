# 🔍 Troubleshooting - Solusi Masalah Claude Code

**Dibuat oleh [DigitalHub Indonesia](https://github.com/mas-mikel22)**

Panduan lengkap untuk mengatasi berbagai error yang mungkin terjadi saat install dan menggunakan Claude Code CLI.

## 📑 Daftar Isi

- [Error Instalasi](#-error-instalasi)
  - [npm is not recognized](#error-npm-is-not-recognized)
  - [npm.ps1 cannot be loaded](#error-npmps1-cannot-be-loaded-execution-policy)
  - [Error Network/Connection (ENOTFOUND)](#error-networkconnection-enotfound)
  - [Permission Denied saat Install](#permission-denied-saat-install)
- [Error Konfigurasi](#-error-konfigurasi)
  - [Konfigurasi tidak bekerja](#konfigurasi-tidak-bekerja)
  - [Claude Code Dialihkan ke Anthropic Console](#claude-code-dialihkan-ke-anthropic-console-login)
- [Error Runtime](#-error-runtime)
  - [Claude Code requires git-bash](#error-claude-code-on-windows-requires-git-bash)
  - [Claude Code tidak bisa akses file](#claude-code-tidak-bisa-akses-file)
  - [claude: command not found](#error-claude-command-not-found)
  - [Error Rate Limit Exceeded](#error-rate-limit-exceeded)
  - [Error Context Window Limit](#error-the-model-has-reached-its-context-window-limit)
  - [Error Auto-update failed](#error-auto-update-failed)
  - [Error 401 Invalid bearer token](#error-401-invalid-bearer-token)
- [Konfigurasi VS Code Extension](#-konfigurasi-vs-code-extension)
  - [Setup Claude Code Extension untuk VS Code](#setup-claude-code-extension-untuk-vs-code)

---

## 🚫 Error Instalasi

### Error "npm is not recognized"

**Error:**
```
npm : The term 'npm' is not recognized as the name of a cmdlet, function, script file, or operable program.
```

**Penyebab:**
Node.js belum terinstall atau npm tidak ada di PATH sistem.

**Solusi:**

1. **Install Node.js:**
   - Download Node.js dari: https://nodejs.org
   - Pilih versi **LTS (Long Term Support)** - direkomendasikan
   - Install dengan default settings
   - **Penting:** Pastikan pilih opsi **"Add to PATH"** saat install

2. **Setelah install, restart terminal:**
   - Tutup semua terminal/PowerShell yang terbuka
   - Buka terminal/PowerShell baru

3. **Verifikasi instalasi:**
   ```powershell
   node --version
   npm --version
   ```
   
   Jika muncul versi (contoh: `v20.10.0` dan `10.2.3`), berarti Node.js sudah terinstall dengan benar.

4. **Install Claude Code CLI:**
   ```powershell
   npm install -g @anthropic-ai/claude-code
   ```

**Catatan Penting:**
- ✅ Node.js versi **18 atau lebih baru** diperlukan
- ✅ Jika sudah install tapi masih error, kemungkinan Node.js tidak ada di PATH
- ✅ **Restart terminal** setelah install Node.js (penting!)
- ✅ Jika masih error setelah restart, cek PATH environment variable

**Cek PATH (Jika Masih Error):**

1. Buka System Properties → Environment Variables
2. Cek apakah ada `C:\Program Files\nodejs\` di PATH
3. Jika tidak ada, tambahkan manual

---

### 🪟 Troubleshooting Khusus Windows

Berikut adalah masalah-masalah umum yang sering terjadi di Windows saat instalasi Node.js dan Claude Code:

#### Masalah 1: Node.js Terinstall Tapi Command Tidak Ditemukan

**Gejala:**
```
node : The term 'node' is not recognized...
npm : The term 'npm' is not recognized...
```

**Penyebab:**
- Node.js sudah terinstall di `C:\Program Files\nodejs\` tapi PATH belum terupdate di terminal yang sedang terbuka
- Terminal/VS Code dibuka sebelum instalasi Node.js selesai

**Solusi:**

1. **Tutup SEMUA terminal dan VS Code yang terbuka**
2. **Buka terminal/VS Code baru**
3. Test lagi:
   ```cmd
   node -v
   npm -v
   ```

**Jika masih tidak berfungsi**, cek apakah Node.js benar-benar terinstall:

```powershell
# Cek apakah file node.exe ada
Test-Path "C:\Program Files\nodejs\node.exe"

# Jika True, coba jalankan langsung dengan full path
& "C:\Program Files\nodejs\node.exe" -v
& "C:\Program Files\nodejs\npm.cmd" -v
```

**Jika full path berfungsi tapi command biasa tidak**, berarti masalah di PATH. Lanjut ke solusi berikut.

---

#### Masalah 2: PATH Tidak Terbaca di PowerShell/Terminal

**Gejala:**
- `node -v` dengan full path berfungsi
- `node -v` tanpa path tidak berfungsi
- Setelah restart terminal masih sama

**Solusi: Refresh PATH di PowerShell**

```powershell
# Refresh environment variables tanpa restart
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")

# Test lagi
node -v
npm -v
```

**Solusi: Tambah PATH Manual (Permanen)**

Jika cara di atas tidak berhasil, tambah PATH secara manual:

1. Tekan **Win + R**
2. Ketik: `rundll32.exe sysdm.cpl,EditEnvironmentVariables`
3. Enter
4. Di **User variables** (atas), cari **"Path"**
5. Klik **Edit**
6. Klik **New**
7. Tambahkan: `C:\Program Files\nodejs\`
8. Klik **OK** semua
9. **Restart semua terminal dan VS Code**
10. Test lagi

---

#### Masalah 3: PowerShell Execution Policy Error

**Gejala:**
```
npm : File C:\Program Files\nodejs\npm.ps1 cannot be loaded because running scripts is disabled on this system.
```

**Penyebab:**
- `node -v` berfungsi tapi `npm -v` tidak
- PowerShell memblokir eksekusi script npm

**Solusi 1: Set Execution Policy (Recommended)**

Buka PowerShell **sebagai Administrator**:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Ketik **Y** untuk konfirmasi, lalu restart PowerShell.

**Solusi 2: Gunakan Command Prompt (CMD)**

Jika tidak bisa ubah Execution Policy, gunakan CMD:

1. Tekan **Win + R**
2. Ketik: `cmd`
3. Enter
4. Test:
   ```cmd
   node -v
   npm -v
   ```

CMD tidak terpengaruh Execution Policy, jadi lebih stabil untuk npm commands.

---

#### Masalah 4: VS Code Terminal Tidak Membaca PATH

**Gejala:**
- `node` dan `npm` berfungsi di CMD/PowerShell standalone
- Tidak berfungsi di terminal VS Code

**Solusi:**

1. **Tutup VS Code sepenuhnya** (penting!)
2. **Buka VS Code baru**
3. Buka terminal di VS Code (Ctrl + `)
4. Test:
   ```cmd
   node -v
   npm -v
   ```

**Jika masih tidak berfungsi:**

1. Tekan **Ctrl + Shift + P**
2. Ketik: `Developer: Reload Window`
3. Enter
4. Test lagi di terminal

**Alternatif: Ubah Default Terminal ke CMD**

1. Tekan **Ctrl + Shift + P**
2. Ketik: `Terminal: Select Default Profile`
3. Pilih **Command Prompt**
4. Buka terminal baru (Ctrl + `)

---

#### Masalah 5: `claude` Command Not Found Setelah Install

**Gejala:**
```
claude : The term 'claude' is not recognized...
```

**Penyebab:**
- npm global packages disimpan di `%APPDATA%\npm` yang belum ada di PATH
- Executable yang terinstall adalah `claude` bukan `claude-code`

**Solusi:**

1. **Cek lokasi npm global packages:**
   ```cmd
   npm config get prefix
   ```
   
   Biasanya: `C:\Users\USER\AppData\Roaming\npm`

2. **Tambah ke PATH:**
   - Tekan **Win + R**
   - Ketik: `rundll32.exe sysdm.cpl,EditEnvironmentVariables`
   - Di **User variables**, edit **"Path"**
   - Klik **New**
   - Tambahkan path dari step 1 (contoh: `C:\Users\USER\AppData\Roaming\npm`)
   - Klik **OK** semua

3. **Restart semua terminal dan VS Code**

4. **Test:**
   ```cmd
   claude --version
   ```

**Catatan:** Command yang benar adalah `claude`, bukan `claude-code`.

---

#### Masalah 6: Tidak Punya Akses Administrator

**Gejala:**
- Tidak bisa edit System variables (bagian bawah)
- Hanya bisa edit User variables (bagian atas)

**Solusi:**

**Tidak masalah!** User variables sudah cukup:

1. Di **User variables** (atas), edit **"Path"**
2. Tambahkan:
   - `C:\Program Files\nodejs\`
   - `C:\Users\USER\AppData\Roaming\npm` (sesuaikan dengan username Anda)
3. Klik **OK**
4. Restart terminal

User PATH hanya berlaku untuk user Anda, tapi itu sudah cukup untuk development.

---

#### Ringkasan Solusi Windows

| Masalah | Solusi Cepat |
|---------|--------------|
| `node/npm not recognized` | Restart terminal/VS Code |
| PATH tidak terbaca | Tambah manual di User variables |
| PowerShell Execution Policy | Gunakan CMD atau set RemoteSigned |
| VS Code terminal error | Tutup VS Code sepenuhnya, buka lagi |
| `claude not found` | Tambah `%APPDATA%\npm` ke PATH |
| Tidak ada akses Admin | Gunakan User variables (sudah cukup) |

**Langkah Umum yang Selalu Berhasil:**
1. ✅ Install Node.js dari nodejs.org
2. ✅ Tambah `C:\Program Files\nodejs\` ke User PATH
3. ✅ Tambah `C:\Users\USER\AppData\Roaming\npm` ke User PATH
4. ✅ Restart semua terminal dan VS Code
5. ✅ Gunakan CMD jika PowerShell bermasalah
6. ✅ Test dengan `node -v`, `npm -v`, `claude --version`

---

### Error "npm.ps1 cannot be loaded" (Execution Policy)

**Error:**
```
npm.ps1 cannot be loaded because running scripts is disabled on this system.
For more Information, see about_Execution_Policies
```

**Penyebab:**
PowerShell execution policy memblokir npm script, meskipun Node.js sudah terinstall dengan benar.

**Solusi 1: Set Execution Policy (Direkomendasikan)**

1. **Buka PowerShell sebagai Administrator:**
   - Klik kanan PowerShell → "Run as Administrator"

2. **Set Execution Policy:**
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```

3. **Konfirmasi dengan "Y"** jika diminta

4. **Tutup PowerShell Administrator**, buka PowerShell biasa

5. **Install Claude Code CLI:**
   ```powershell
   npm install -g @anthropic-ai/claude-code
   ```

**Solusi 2: Gunakan Command Prompt (CMD)**

CMD tidak terpengaruh execution policy, jadi bisa langsung digunakan:

1. **Buka Command Prompt (CMD)**
2. **Jalankan:**
   ```cmd
   npm install -g @anthropic-ai/claude-code
   ```

**Catatan:**
- Error ini muncul meskipun Node.js sudah terinstall dengan benar
- Execution policy hanya mempengaruhi PowerShell, bukan CMD
- Solusi 2 (CMD) lebih cepat jika tidak ingin mengubah execution policy

---

### Error Network/Connection (ENOTFOUND)

**Error:**
```
npm error code ENOTFOUND
npm error network request to https://registry.npmjs.org/@anthropic-ai/claude-code failed
npm error network This is a problem related to network connectivity.
npm error network In most cases you are behind a proxy or have bad network settings.
```

**Penyebab:**
- Masalah koneksi internet
- Terhubung ke proxy yang memblokir npm registry
- DNS tidak bisa resolve `registry.npmjs.org`
- Firewall atau antivirus memblokir koneksi

**Solusi 1: Cek Koneksi Internet**

1. **Pastikan internet terhubung:**
   - Coba buka website lain di browser
   - Coba buka https://registry.npmjs.org di browser
   - Jika tidak bisa dibuka, ada masalah koneksi

2. **Coba install lagi:**
   ```powershell
   npm install -g @anthropic-ai/claude-code
   ```
   Terkadang masalah sementara, coba lagi bisa berhasil.

**Solusi 2: Ganti DNS**

Jika DNS tidak bisa resolve `registry.npmjs.org`:

1. **Buka Network Settings:**
   - Settings → Network & Internet → Change adapter options
   - Klik kanan network adapter → Properties
   - Pilih "Internet Protocol Version 4 (TCP/IPv4)" → Properties

2. **Ganti DNS:**
   - Pilih "Use the following DNS server addresses"
   - Preferred: `8.8.8.8` (Google DNS)
   - Alternate: `8.8.4.4` (Google DNS)
   - Atau gunakan Cloudflare: `1.1.1.1` dan `1.0.0.1`

3. **Restart komputer** dan coba install lagi

**Solusi 3: Konfigurasi Proxy (Jika Menggunakan Proxy)**

Jika Anda menggunakan proxy:

```powershell
npm config set proxy http://proxy-server:port
npm config set https-proxy http://proxy-server:port
```

**Solusi 4: Gunakan Registry Mirror (Alternatif)**

Jika registry npm tidak bisa diakses, gunakan mirror:

```powershell
npm install -g @anthropic-ai/claude-code --registry https://registry.npmmirror.com
```

**Solusi 5: Cek Firewall/Antivirus**

1. **Cek Windows Firewall:**
   - Pastikan tidak memblokir npm
   - Atau coba disable sementara untuk testing

2. **Cek Antivirus:**
   - Beberapa antivirus memblokir npm
   - Coba disable sementara (untuk testing)
   - Atau tambahkan exception untuk npm

**Catatan:**
- Error ini bukan masalah instalasi Node.js atau execution policy
- Masalah murni koneksi jaringan ke npm registry
- Solusi 2 (Ganti DNS) sering membantu jika masalah DNS

---

### Permission Denied saat Install

**Windows:**
- Jalankan PowerShell sebagai Administrator
- Atau install manual: `npm install -g @anthropic-ai/claude-code`

**Mac/Linux:**
- Gunakan `sudo`: `sudo npm install -g @anthropic-ai/claude-code`

---

## ⚙️ Error Konfigurasi

### Konfigurasi tidak bekerja

**Error:** "API key tidak ditemukan" atau Claude Code tidak bisa connect

**Solusi:**
1. **Tutup semua terminal/Claude Code** dan buka terminal baru
2. **Cek environment variables:**
   - Windows: `echo $env:ANTHROPIC_AUTH_TOKEN`
   - Mac/Linux: `echo $ANTHROPIC_AUTH_TOKEN`
3. **Jika kosong**, setup ulang dengan `setx` atau script otomatis
4. **Pastikan tutup terminal dan buka terminal baru** setelah setup



### Claude Code Dialihkan ke Anthropic Console (Login)

**Error:** 
Setelah memilih opsi "2. Anthropic Console account · API usage billing", Claude Code membuka browser dan mengarahkan ke halaman login Anthropic Console, padahal kita ingin menggunakan API key yang sudah di setting di environment variables.

**Penyebab:**
Claude Code secara default akan mencoba login ke Anthropic Console untuk mendapatkan API key Anthropic. Karena kita menggunakan API key yang sudah di setting di environment variables login tersebut tidak diperlukan.

**Solusi: Buat/Edit File Settings.json**

Agar Claude Code langsung menggunakan environment variables tanpa login Anthropic, buat atau edit file `~/.claude/settings.json`:

**Windows:**
```powershell
# Buat folder jika belum ada
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"

# Buat/edit file settings.json
@"
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic"
  }
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding utf8
```

**Catatan:** Ganti `your_api_key` dengan API key Anda yang sebenarnya.

**Mac/Linux:**
```bash
# Buat folder jika belum ada
mkdir -p ~/.claude

# Buat/edit file settings.json
cat > ~/.claude/settings.json << 'EOF'
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic"
  }
}
EOF
```

**Catatan:** Ganti `your_api_key` dengan API key Anda yang sebenarnya.

**Setelah membuat file settings.json:**

1. **Tutup/Decline** halaman login Anthropic Console (jika masih terbuka)
2. **Tutup terminal** dan buka terminal baru
3. **Jalankan `claude` lagi** - CLI akan langsung menggunakan API key dari settings.json tanpa perlu login

**Alternatif: Gunakan Environment Variables Saja**

Jika tidak ingin membuat file settings.json, pastikan environment variables sudah di-set dengan benar:

**Windows:**
```cmd
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
```

**Mac/Linux:**
```bash
export ANTHROPIC_AUTH_TOKEN=your_api_key
export ANTHROPIC_BASE_URL=https://api.z.ai/api/anthropic
```

**Catatan Penting:**
- ✅ File `settings.json` akan membuat Claude Code langsung menggunakan API key tanpa login
- ✅ Environment variables juga bisa digunakan, tapi harus di-set setiap kali buka terminal baru (kecuali menggunakan `setx` di Windows)
- ✅ Jika sudah membuat `settings.json`, tidak perlu login ke Anthropic Console lagi

---

## 🚀 Error Runtime

### Error "Claude Code on Windows requires git-bash"

**Error:**
```
Error: Claude Code on Windows requires git-bash
If installed but not in PATH, set environment variable pointing to your bash.exe
```

**Penyebab:**
Claude Code di Windows memerlukan Git Bash untuk berjalan, tapi Git Bash tidak ditemukan atau tidak ada di PATH.

**Solusi 1: Install Git Bash (Jika Belum Terinstall)**

1. **Download Git untuk Windows:**
   - Kunjungi: https://git-scm.com/downloads/win
   - Download dan install Git untuk Windows
   - **Penting:** Saat install, pastikan pilih opsi **"Add Git to PATH"**

2. **Setelah install, restart terminal** dan jalankan `claude` lagi

**Solusi 2: Set Environment Variable (Jika Git Bash Sudah Terinstall)**

Jika Git Bash sudah terinstall tapi tidak di PATH:

1. **Cari lokasi Git Bash:**
   - Biasanya ada di: `C:\Program Files\Git\bin\bash.exe`
   - Atau: `C:\Program Files (x86)\Git\bin\bash.exe`
   - Atau cek dengan: `where bash` di Command Prompt

2. **Set environment variable:**
   ```cmd
   setx CLAUDE_CODE_GIT_BASH_PATH "C:\Program Files\Git\bin\bash.exe"
   ```
   
   **Catatan:** Ganti path dengan lokasi Git Bash Anda yang sebenarnya

3. **Tutup terminal dan buka terminal baru**

4. **Jalankan `claude` lagi**

**Cek Apakah Git Bash Terinstall:**

Jalankan di Command Prompt atau PowerShell:
```cmd
where bash
```

Jika muncul path seperti `C:\Program Files\Git\bin\bash.exe`, berarti Git Bash sudah terinstall.

**Cara Cek Lebih Lengkap (PowerShell):**

Anda bisa menjalankan script singkat ini di PowerShell untuk memastikan:

```powershell
# Cek apakah bash ada di PATH atau di lokasi default
if (Get-Command bash -ErrorAction SilentlyContinue) { 
    Write-Host "✅ Git Bash DITEMUKAN di PATH" -ForegroundColor Green
    Get-Command bash | Select-Object Source
} elseif (Test-Path "C:\Program Files\Git\bin\bash.exe") {
    Write-Host "✅ Git Bash DITEMUKAN di C:\Program Files\Git\bin\bash.exe" -ForegroundColor Green
    Write-Host "⚠️ TAPI tidak ada di PATH. Perlu setting CLAUDE_CODE_GIT_BASH_PATH" -ForegroundColor Yellow
} else {
    Write-Host "❌ Git Bash TIDAK DITEMUKAN. Harap install Git for Windows." -ForegroundColor Red
}
```

---

### Claude Code tidak bisa akses file

- Pastikan Anda menjalankan `claude` di dalam folder project
- Grant permission saat diminta saat pertama kali
- Cek permission folder project

---

### Error "claude: command not found"

**Error:**
```
claude : The term 'claude' is not recognized as the name of a cmdlet, function, script file, or operable program.
```

**Penyebab:**
- ❌ **BUKAN masalah PowerShell** - PowerShell bekerja dengan baik
- ✅ **Masalah:** Claude Code CLI belum terinstall, atau npm global bin tidak ada di PATH

**Solusi Step-by-Step:**

#### Step 1: Cek Apakah Claude Code Sudah Terinstall

Jalankan di PowerShell:
```powershell
npm list -g @anthropic-ai/claude-code
```

**Jika muncul error atau "empty", berarti belum terinstall.** Lanjut ke Step 2.

**Jika muncul versi (contoh: `@anthropic-ai/claude-code@2.0.14`), berarti sudah terinstall.** Lanjut ke Step 3.

#### Step 2: Install Claude Code CLI

Jika belum terinstall, install dengan:

```powershell
npm install -g @anthropic-ai/claude-code
```

**Catatan:**
- Jika muncul error `npm is not recognized`, lihat bagian [Error "npm is not recognized"](#error-npm-is-not-recognized)
- Jika muncul error `npm.ps1 cannot be loaded`, lihat bagian [Error "npm.ps1 cannot be loaded"](#error-npmps1-cannot-be-loaded-execution-policy)
- Jika muncul error network, lihat bagian [Error Network/Connection (ENOTFOUND)](#error-networkconnection-enotfound)

#### Step 3: Restart Terminal (PENTING!)

**Setelah install, WAJIB restart terminal:**

1. **Tutup semua terminal/PowerShell yang terbuka**
2. **Buka PowerShell baru**
3. **Coba jalankan `claude` lagi**

**Mengapa harus restart?**
- Terminal yang sudah terbuka tidak akan membaca PATH yang baru
- Terminal baru akan membaca PATH terbaru dari sistem

#### Step 4: Cek PATH npm Global Bin (Jika Masih Error)

Jika setelah restart masih error, cek apakah npm global bin ada di PATH:

**Windows PowerShell:**
```powershell
npm config get prefix
```

Ini akan menampilkan path seperti: `C:\Users\[USERNAME]\AppData\Roaming\npm`

**Cek apakah path tersebut ada di PATH:**
```powershell
$env:PATH -split ';' | Select-String "npm"
```

**Jika tidak muncul**, tambahkan manual ke PATH:

1. Buka System Properties → Environment Variables
2. Edit "Path" di User variables
3. Tambahkan path dari `npm config get prefix` (contoh: `C:\Users\[USERNAME]\AppData\Roaming\npm`)
4. **Restart terminal** setelah menambah PATH

#### Step 5: Verifikasi Instalasi

Setelah semua langkah di atas, verifikasi:

```powershell
claude --version
```

Jika muncul versi (contoh: `2.0.14`), berarti sudah berhasil!

**Mac/Linux:**

1. **Cek apakah npm global bin ada di PATH:**
   ```bash
   echo $PATH
   ```
   Pastikan ada path seperti `/usr/local/bin` atau `/home/[USERNAME]/.npm-global/bin`

2. **Install ulang dengan `sudo` jika perlu:**
   ```bash
   sudo npm install -g @anthropic-ai/claude-code
   ```

3. **Restart terminal** setelah install

**Catatan Penting:**
- ✅ **PowerShell tidak bermasalah** - Error ini normal jika CLI belum terinstall
- ✅ **Restart terminal** adalah langkah penting yang sering terlewat
- ✅ Jika sudah install tapi masih error, kemungkinan masalah PATH

---

### Error "Rate Limit Exceeded"

- Tunggu beberapa detik, lalu coba lagi
- Jangan spam chat (tunggu response selesai sebelum kirim berikutnya)
- Lihat [`PENJELASAN_RATE_LIMIT.md`](PENJELASAN_RATE_LIMIT.md) untuk penjelasan lengkap

---

### Error "The model has reached its context window limit"

**Error:**
```
API Error: The model has reached its context window limit
```

**Penyebab:**
- **Percakapan terlalu panjang** - Setiap model AI memiliki batas maksimal "context window" (jumlah token/karakter yang bisa diproses sekaligus)
- **Terlalu banyak file/kode** yang dibaca dalam satu sesi
- **Akumulasi pesan** - Semakin lama percakapan, semakin banyak token yang terakumulasi
- **Meminta AI membaca seluruh codebase** sekaligus

**Solusi 1: Mulai Percakapan Baru (Tercepat)**

Ketik `/reset` atau mulai conversation baru untuk mereset context dan membebaskan token.

```
/reset
```

**Solusi 2: Gunakan /compact (Jika Tersedia)**

Beberapa tools memiliki fitur untuk meringkas percakapan:

```
/compact
```

**Solusi 3: Pecah Tugas Menjadi Lebih Kecil**

- Alih-alih meminta AI memproses banyak file sekaligus, minta satu per satu
- Fokus pada satu fitur/fungsi per percakapan
- Hindari meminta AI membaca seluruh codebase dalam satu perintah

**Tips Mencegah Error Ini:**

| Tips | Penjelasan |
|------|------------|
| Mulai percakapan baru untuk task baru | Jangan lanjutkan percakapan yang sudah panjang untuk task berbeda |
| Fokus pada file spesifik | Sebutkan file yang relevan saja, bukan "baca semua" |
| Pecah refactoring besar | Untuk perubahan besar, pecah menjadi beberapa percakapan |
| Gunakan `/compact` berkala | Jika tersedia, ringkas percakapan sebelum terlalu panjang |

**Catatan Penting:**
- ✅ Error ini **bukan masalah API key atau koneksi** - ini murni batas kapasitas model
- ✅ Data Anda **tidak hilang** - cukup mulai percakapan baru
- ✅ Beberapa model memiliki context window lebih besar dari yang lain
- ⚠️ Percakapan yang sangat panjang (>2 jam coding intensif) sering mencapai batas ini

---

### Error "Auto-update failed"

**Error:**
```
X Auto-update failed · Try claude doctor or npm i -g @anthropic-ai/claude-code
```

**Penyebab:**
- Claude Code gagal melakukan auto-update ke versi terbaru
- Biasanya disebabkan oleh masalah koneksi internet, permission, atau masalah dengan npm registry

**Solusi 1: Jalankan claude doctor (Direkomendasikan)**

Jalankan command berikut untuk diagnosa masalah:

```powershell
claude doctor
```

Command ini akan menampilkan informasi tentang:
- Versi Claude Code yang terinstall
- Status environment variables
- Masalah yang terdeteksi
- Saran perbaikan

**Solusi 2: Install Ulang Claude Code**

Jika `claude doctor` tidak membantu, install ulang Claude Code:

```powershell
npm install -g @anthropic-ai/claude-code
```

**Catatan:** 
- Jika muncul error `npm is not recognized`, lihat bagian [Error "npm is not recognized"](#error-npm-is-not-recognized)
- Jika muncul error network, lihat bagian [Error Network/Connection (ENOTFOUND)](#error-networkconnection-enotfound)

**Solusi 3: Install dengan Versi Spesifik**

Jika auto-update terus gagal, install versi spesifik yang stabil:

```powershell
npm install -g @anthropic-ai/claude-code@latest
```

**Solusi 4: Cek Koneksi Internet**

Auto-update memerlukan koneksi internet. Pastikan:
- Internet terhubung dengan baik
- Tidak ada firewall yang memblokir npm registry
- DNS bisa resolve `registry.npmjs.org`

**Catatan Penting:**
- ✅ Error ini **tidak menghalangi** penggunaan Claude Code - Anda masih bisa menggunakan versi yang terinstall
- ✅ Auto-update adalah fitur opsional untuk mendapatkan versi terbaru
- ✅ Jika tidak urgent, bisa diabaikan dan update manual nanti
- ⚠️ Jika error terus muncul, kemungkinan ada masalah dengan instalasi atau koneksi

---

### Error "401 Invalid bearer token"

**Error:**
```
LAPI Error: 401
{"type": "error", "error":{"type": "authentication_error", "message": "Invalid bearer token"}, "request_id":"req_..."}
Please run /login
```

**Penyebab:**
- API key tidak ditemukan atau tidak valid
- API key salah atau sudah expired
- Claude Code tidak membaca environment variables atau settings.json dengan benar
- Konfigurasi API key tidak sesuai dengan endpoint yang digunakan

**Solusi 1: Cek Environment Variables**

Pastikan environment variables sudah di-set dengan benar:

**Windows PowerShell:**
```powershell
echo $env:ANTHROPIC_AUTH_TOKEN
echo $env:ANTHROPIC_BASE_URL
```

**Mac/Linux:**
```bash
echo $ANTHROPIC_AUTH_TOKEN
echo $ANTHROPIC_BASE_URL
```

**Jika kosong atau salah**, set ulang:

**Windows (CMD atau PowerShell):**
```cmd
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
```

**Mac/Linux:**

Untuk membuat environment variables permanen, tambahkan ke file `~/.bashrc` atau `~/.zshrc` (tergantung shell yang digunakan):

```bash
# Buka file dengan editor (pilih salah satu sesuai shell Anda)
nano ~/.bashrc
# atau
nano ~/.zshrc

# Tambahkan baris berikut di akhir file:
export ANTHROPIC_AUTH_TOKEN=your_api_key
export ANTHROPIC_BASE_URL=https://api.z.ai/api/anthropic

# Simpan dan keluar (Ctrl+X, lalu Y, lalu Enter untuk nano)
# Reload shell configuration:
source ~/.bashrc
# atau
source ~/.zshrc
```

**Catatan:** Ganti `your_api_key` dengan API key Anda yang sebenarnya.

**Setelah set ulang:**
1. **Tutup semua terminal/Claude Code**
2. **Buka terminal baru**
3. **Jalankan `claude` lagi**

**Solusi 2: Cek dan Perbaiki File settings.json**

Jika environment variables sudah benar tapi masih error, cek file `~/.claude/settings.json`:

**Windows:**
```powershell
# Baca isi file settings.json
Get-Content $env:USERPROFILE\.claude\settings.json
```

**Mac/Linux:**
```bash
# Baca isi file settings.json
cat ~/.claude/settings.json
```

**Jika file tidak ada atau isinya salah**, buat/edit file:

**Windows:**
```powershell
# Buat folder jika belum ada
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude"

# Buat/edit file settings.json
@"
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_api_key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic"
  }
}
"@ | Out-File -FilePath "$env:USERPROFILE\.claude\settings.json" -Encoding utf8
```

**Mac/Linux:**
```bash
# Buat folder jika belum ada
mkdir -p ~/.claude

# Buat/edit file settings.json
cat > ~/.claude/settings.json << 'EOF'
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_api_key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic"
  }
}
EOF
```

**Catatan:** Ganti `your_api_key` dengan API key Anda yang sebenarnya.

**Setelah membuat/edit file:**
1. **Tutup semua terminal/Claude Code**
2. **Buka terminal baru**
3. **Jalankan `claude` lagi**

**Solusi 3: Hapus Konfigurasi Lama dan Setup Ulang**

Jika masih error, hapus konfigurasi lama dan setup ulang:

**Windows:**
```powershell
# Hapus file settings.json
del $env:USERPROFILE\.claude\settings.json

# Set environment variables ulang
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
```

**Mac/Linux:**
```bash
# Hapus file settings.json
rm ~/.claude/settings.json

# Set environment variables ulang (tambahkan ke ~/.bashrc atau ~/.zshrc)
echo 'export ANTHROPIC_AUTH_TOKEN=your_api_key' >> ~/.bashrc
echo 'export ANTHROPIC_BASE_URL=https://api.z.ai/api/anthropic' >> ~/.bashrc

# Reload shell configuration
source ~/.bashrc
```

**Catatan:** 
- Untuk Mac/Linux, jika menggunakan zsh, ganti `~/.bashrc` dengan `~/.zshrc`
- Ganti `your_api_key` dengan API key Anda yang sebenarnya

**Setelah itu:**
1. **Tutup semua terminal**
2. **Buka terminal baru**
3. **Jalankan `claude` lagi**

**Solusi 4: Verifikasi API Key**

Pastikan API key yang digunakan adalah API key yang valid:

1. **Pastikan tidak ada spasi** di awal atau akhir API key saat copy-paste
2. **Pastikan API key lengkap** (tidak terpotong)

**Catatan Penting:**
- ✅ Error 401 berarti **authentication gagal** - API key tidak valid atau tidak ditemukan
- ✅ **Environment variables** harus di-set dengan `setx` di Windows atau ditambahkan ke `~/.bashrc`/`~/.zshrc` di Mac/Linux agar permanen
- ✅ **File `settings.json`** akan override environment variables jika ada
- ✅ **Restart terminal** sangat penting setelah mengubah environment variables atau settings.json
- ⚠️ Jika masih error setelah semua solusi, kemungkinan API key tidak valid atau sudah expired
- ⚠️ Di Mac/Linux, pastikan menggunakan shell yang benar (`bash` atau `zsh`) saat menambahkan environment variables

---

## 🔧 Konfigurasi VS Code Extension
### Setup Claude Code Extension untuk VS Code

Untuk menggunakan **Claude Code extension** di VS Code dengan API, cukup setup konfigurasi Claude Code.

> [!TIP]
> **Kabar Baik!** Setup Claude Code berlaku untuk **CLI dan VS Code Extension** sekaligus!
> Jadi Anda hanya perlu setup sekali untuk kedua-duanya.

> [!IMPORTANT]
> **Jika Anda sudah setup konfigurasi Claude Code, Anda tidak perlu melakukan apa-apa lagi!**
> VS Code Extension akan otomatis bekerja.

---

**Setup Konfigurasi:**

**Windows (CMD atau PowerShell):**
```cmd
setx ANTHROPIC_AUTH_TOKEN your_api_key
setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
```

**Mac/Linux - Automated Script (Direkomendasikan):**
```bash
curl -O "https://cdn.bigmodel.cn/install/claude_code_zai_env.sh" && bash claude_code_zai_env.sh
```

**Mac/Linux - Manual Configuration:**

Edit file `~/.claude/settings.json`:
```json
{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "your_zai_api_key",
    "ANTHROPIC_BASE_URL": "https://api.z.ai/api/anthropic",
    "API_TIMEOUT_MS": "3000000"
  }
}
```

---

**Langkah selanjutnya:**
1. Ganti `your_api_key` / `your_zai_api_key` dengan API key Anda yang sebenarnya
2. **Tutup VS Code sepenuhnya** dan buka kembali
3. **Selesai!** ✅

**Hasil:**
- ✅ Claude Code CLI langsung menggunakan API
- ✅ Claude Code VS Code Extension juga otomatis bekerja
- ✅ **Satu setup untuk CLI dan Extension!**

---

**Troubleshooting:**

- ⚠️ **JANGAN** gunakan `CLAUDE_CODE_SKIP_AUTH_LOGIN` - menyebabkan loading loop "Noodling..."
- ⚠️ Pastikan API key valid dan tidak ada spasi di awal/akhir
- ⚠️ **Restart VS Code sepenuhnya** (tutup dan buka lagi) setelah setup
- ⚠️ Jika masih tidak bekerja, cek konfigurasi:
  - Windows: `echo %ANTHROPIC_AUTH_TOKEN%` (CMD) atau `echo $env:ANTHROPIC_AUTH_TOKEN` (PowerShell)
  - Mac/Linux: Cek file `~/.claude/settings.json`


### VS Code Extension Panel Loading Terus (Wibbling...)

**Gejala:**
- Panel Claude Code extension di sidebar menampilkan "Wibbling..." atau loading terus menerus
- Loading tidak selesai meskipun sudah menunggu lama (lebih dari 1 menit)
- Settings.json sudah dikonfigurasi dengan benar
- CLI (`claude` command) berfungsi dengan baik di terminal

**Penyebab:**
- Bug di Claude Code extension untuk Windows
- Incompatibility dengan versi VS Code tertentu
- Conflict dengan extension lain
- Cache VS Code bermasalah

**Solusi 1: Gunakan CLI di Terminal (Recommended)**

Karena CLI sudah berfungsi dengan sempurna, gunakan CLI sebagai alternatif yang lebih stabil:

**Cara 1: Centang "Use Terminal" di Extension Settings**

1. Buka Settings (Ctrl+,)
2. Cari: `Claude Code: Use Terminal`
3. **Centang** checkbox
4. Sekarang saat klik ikon Claude Code di sidebar, akan otomatis membuka CLI di terminal

**Cara 2: Manual di Terminal**

1. Buka terminal di VS Code (Ctrl + `)
2. Jalankan: `claude`
3. Chat langsung di terminal

**Keuntungan CLI:**
- ✅ Lebih cepat (tidak ada loading UI)
- ✅ Lebih stabil (tidak ada bug panel UI)
- ✅ Fitur lengkap sama dengan panel UI
- ✅ Sudah terbukti berfungsi dengan baik

**Solusi 2: Reinstall Extension (Jika Tetap Ingin Panel UI)**

Jika tetap ingin menggunakan panel UI:

1. **Uninstall extension:**
   - Buka Extensions (Ctrl+Shift+X)
   - Cari "Claude Code"
   - Klik **Uninstall**

2. **Clear VS Code cache:**
   ```cmd
   rd /s /q "%APPDATA%\Code\Cache"
   rd /s /q "%APPDATA%\Code\CachedData"
   ```

3. **Restart VS Code sepenuhnya** (tutup dan buka lagi)

4. **Install ulang extension:**
   - Buka Extensions
   - Cari "Claude Code"
   - Install

5. **Restart VS Code lagi**

6. **Coba buka panel Claude Code**

**Solusi 3: Cek Output Panel untuk Error**

Lihat error spesifik yang mungkin muncul:

1. Tekan **Ctrl + Shift + U** (Output panel)
2. Di dropdown, pilih **"Claude Code"**
3. Screenshot atau copy error message yang muncul
4. Report ke Anthropic support jika ada bug

**Solusi 4: Update VS Code**

1. **Cek versi VS Code:** Help → About
2. **Update VS Code** jika ada versi baru
3. **Restart VS Code**
4. **Coba panel lagi**

**Catatan Penting:**
- ✅ **CLI adalah solusi yang valid**, bukan workaround sementara
- ✅ Banyak developer prefer CLI karena lebih cepat dan stabil
- ✅ Jika panel UI loading terus setelah semua solusi, kemungkinan bug di extension
- ⚠️ Report issue ke Anthropic GitHub jika masalah persisten: https://github.com/anthropics/anthropic-quickstarts

**Perbedaan CLI vs Panel UI:**

| Aspek | CLI (Terminal) | Panel UI (Sidebar) |
|-------|----------------|-------------------|
| **Kecepatan** | ⚡ Instant | 🐌 Loading UI |
| **Stabilitas** | ✅ Sangat stabil | ⚠️ Kadang bug |
| **Fitur** | ✅ Lengkap | ✅ Lengkap |
| **Interface** | Terminal text | Visual panel |
| **Cocok untuk** | Developer yang suka terminal | Developer yang suka UI visual |

**Rekomendasi:**
- Untuk Windows, **gunakan CLI** karena lebih stabil
- Panel UI bisa dicoba lagi setelah update extension di masa depan

**Referensi:**
- Dokumentasi resmi: https://code.claude.com/docs/en/settings
- Extension akan membaca environment variables dari `claudeCode.environmentVariables` di settings.json

---

## ℹ️ Catatan: Keluar dari API Claude

1. Di terminal Claude Code yang sedang aktif, ketik `/exit` atau tekan `Ctrl+C` untuk menutup sesi.
2. Jalankan `claude logout` agar token sebelumnya dihapus.
3. Tutup semua terminal VS Code, lalu buka PowerShell baru.
4. Pastikan environment variable aktif:
   ```powershell
   echo $env:ANTHROPIC_AUTH_TOKEN
   echo $env:ANTHROPIC_BASE_URL
   ```
   - Jika kosong, set ulang:
     ```cmd
     setx ANTHROPIC_AUTH_TOKEN your_api_key
     setx ANTHROPIC_BASE_URL https://api.z.ai/api/anthropic
     ```
5. (Opsional) Hapus konfigurasi lama agar CLI membaca env vars baru:
   ```powershell
   del $env:USERPROFILE\.claude\settings.json
   ```
6. Tutup terminal, buka terminal baru, masuk ke folder project, jalankan `claude`

---

## 📚 Referensi

- [`README.md`](README.md) - Panduan utama dan instalasi lengkap
- [`PENJELASAN_RATE_LIMIT.md`](PENJELASAN_RATE_LIMIT.md) - Penjelasan tentang rate limit

---

## 👤 Tentang DigitalHub Indonesia

**DigitalHub Indonesia** adalah penyedia solusi digital yang berfokus pada pengembangan teknologi AI, otomatisasi, dan konsultasi IT di Indonesia.

**Hubungi kami:**
- GitHub: [@mas-mikel22](https://github.com/mas-mikel22)
- Repository: [panduan-claudeai](https://github.com/mas-mikel22/panduan-claudeai)


