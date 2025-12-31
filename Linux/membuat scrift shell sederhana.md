# **2️⃣ Mengelola Perangkat Lunak (Manage Software – RHEL Style)**

RHEL dan turunannya (**AlmaLinux / Rocky Linux**) menggunakan **DNF**
(`dnf` = _Dandified YUM_, pengganti `yum`).

---

## **2.a Mengonfigurasi Akses Repositori RPM**

### 📌 Penjelasan

- **Repositori** = kumpulan paket RPM
- Konfigurasi repositori disimpan di:

  ```bash
  /etc/yum.repos.d/
  ```

- File repositori berekstensi **`.repo`**
- Sumber repo bisa:

  - Local (`file://`)
  - HTTP / HTTPS
  - FTP

---

### 📌 Struktur File Repository

```ini
[repo-id]
name=Nama Repository
baseurl=URL repository
enabled=1
gpgcheck=0|1
```

---

### 📌 Contoh Repository Lokal

File:

```bash
/etc/yum.repos.d/myrepo.repo
```

Isi:

```ini
[my-local-repo]
name=My Local Repository
baseurl=file:///mnt/cdrom/BaseOS
enabled=1
gpgcheck=0
```

📌 **Catatan penting**

- `enabled=1` → repo aktif
- `gpgcheck=1` → verifikasi keamanan (disarankan di production)
- Untuk RHCSA, **repo lokal sering muncul di soal**

---

### 📌 Cek Repository

```bash
dnf repolist
dnf repolist all
```

---

## **2.b Menginstal, Menghapus, dan Mengelola Paket RPM**

### 📌 Penjelasan

- `dnf` adalah alat utama manajemen paket
- Secara otomatis:

  - Mengelola dependency
  - Mengambil paket dari repo aktif

---

### 📌 Instal Paket

```bash
sudo dnf install httpd
```

📌 Menginstal **Apache Web Server**

---

### 📌 Hapus Paket

```bash
sudo dnf remove httpd
```

---

### 📌 Update Sistem

```bash
sudo dnf update
```

Atau:

```bash
sudo dnf upgrade
```

---

### 📌 Cari Paket

```bash
dnf search firefox
```

---

### 📌 Info Paket

```bash
dnf info httpd
```

---

### 📌 Cek Paket Terinstal

```bash
dnf list installed
dnf list installed httpd
```

---

### 📌 Install RPM Manual

```bash
sudo dnf install file.rpm
```

📌 **Tetap aman** karena dependency tetap dicek.

---

## **2.c Group Package (Tambahan Penting)**

```bash
dnf group list
dnf group install "Development Tools"
```

📌 Sering dipakai untuk install environment lengkap.

---

# **3️⃣ Membuat Skrip Shell Sederhana (Create Simple Shell Scripts)**

Shell script adalah **fondasi automation sysadmin**.

---

## **3.a Dasar Shell Script**

### 📌 Struktur Dasar

```bash
#!/bin/bash
# Komentar
perintah
```

---

### 📌 Membuat Script

```bash
touch script.sh
chmod +x script.sh
```

Jalankan:

```bash
./script.sh
```

---

## **3.b Eksekusi Kondisional (if, test)**

### 📌 Penjelasan

- `if` → pengambilan keputusan
- `test` atau `[ ]` → evaluasi kondisi

---

### 📌 Contoh: Cek File Ada

```bash
#!/bin/bash

FILE="/etc/passwd"

if [ -f "$FILE" ]; then
    echo "$FILE ada."
else
    echo "$FILE tidak ditemukan."
fi
```

---

### 📌 Contoh: Cek Direktori Kosong

```bash
#!/bin/bash

DIR="/tmp/empty_dir"

if [ -d "$DIR" ] && [ -z "$(ls -A $DIR)" ]; then
    echo "Direktori $DIR ada dan kosong."
else
    echo "Direktori tidak ada atau tidak kosong."
fi
```

---

### 📌 Operator Test Penting

| Operator | Arti                |
| -------- | ------------------- |
| -f       | file ada            |
| -d       | direktori ada       |
| -e       | file/direktori ada  |
| -z       | string kosong       |
| -n       | string tidak kosong |

---

## **3.c Looping (for)**

### 📌 Penjelasan

Loop `for` digunakan untuk **mengulang perintah** pada daftar file atau nilai.

---

### 📌 Contoh: Backup File `.txt`

```bash
#!/bin/bash

for file in *.txt
do
    echo "Membackup $file..."
    cp "$file" "$file.backup"
done
```

---

### 📌 Contoh: Loop Daftar User

```bash
#!/bin/bash

for user in user1 user2 user3
do
    echo "User: $user"
done
```

---

## **3.d Praktik Real World SysAdmin**

### 📌 Cek Service & Logging

```bash
#!/bin/bash

SERVICE="httpd"

if systemctl is-active --quiet $SERVICE; then
    echo "$SERVICE aktif"
else
    echo "$SERVICE mati" >> service_error.log
fi
```

---
