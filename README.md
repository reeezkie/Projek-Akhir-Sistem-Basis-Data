# 🏫 Smart School Database - SDN 1 Kota Malang

Proyek Akhir Sistem Basis Data (SBD) - Fakultas Ilmu Komputer, Universitas Brawijaya  
**Kelompok 1:**
1. Anne Mazaya Syafiyah (`245150307111020`)
2. Rezky Auliasarie (`245150300111014`)
3. Zeyra Rahma Alivia (`245150301111016`)

---

## 📌 Latar Belakang
Pengelolaan data yang efisien dan akurat merupakan kebutuhan penting bagi institusi pendidikan seperti **Sekolah Dasar Negeri 1 Kota Malang**. Sebelumnya, sekolah ini masih mengandalkan sistem manual (pencatatan di kertas dan spreadsheet sederhana) yang menyebabkan proses administrasi lambat, rentan terhadap *human error* (seperti salah tulis atau kehilangan data), serta tidak mendukung akses data secara *real-time*.

Dengan mengimplementasikan sistem basis data relasional ini, proyek ini bertujuan untuk membantu SDN 1 Kota Malang mencapai pengelolaan data yang modern, terstruktur, efisien, dan mendukung pengambilan keputusan berbasis data (*data-driven decision making*).

---

## 📊 Model Basis Data & Relasi Entitas (ERD)

Sistem ini terdiri dari 7 entitas utama dengan relasi sebagai berikut:
* **GURU** (1) $\leftrightarrow$ (M) **KELAS** (Wali Kelas)
* **GURU** (M) $\leftrightarrow$ (M) **MATA_PELAJARAN** via tabel **MENGAJAR** $\leftrightarrow$ (M) **KELAS**
* **SISWA** (N) $\leftrightarrow$ (1) **KELAS**
* **SISWA** (N) $\leftrightarrow$ (1) **WALI_SISWA**
* **SISWA** (1) $\leftrightarrow$ (N) **NILAI** $\leftrightarrow$ (1) **MATA_PELAJARAN**
* **SISWA** (1) $\leftrightarrow$ (N) **KEHADIRAN**

---

## 🛠️ Skema SQL & DDL (Oracle / SQL Compatible)

Berikut adalah ringkasan struktur tabel utama yang digunakan dalam proyek ini:

```sql
-- Tabel GURU
CREATE TABLE GURU (
    NIP_Guru INTEGER NOT NULL,
    Nama_Guru VARCHAR2(100),
    Jenis_Kelamin CHAR(3),
    Tanggal_Lahir DATE,
    Alamat VARCHAR2(100),
    No_Telp INTEGER,
    Email VARCHAR2(100),
    Tanggal_Bergabung DATE,
    CONSTRAINT GURU_PK PRIMARY KEY (NIP_Guru)
);

-- Tabel WALI SISWA
CREATE TABLE WALI_SISWA (
    ID_WALISISWA INTEGER NOT NULL,
    Nama_wali VARCHAR2(100),
    Alamat_wali VARCHAR2(100),
    No_Telp_Aktif INTEGER,
    Pekerjaan_wali VARCHAR2(100),
    CONSTRAINT WALI_SISWA_PK PRIMARY KEY (ID_WALISISWA)
);

-- Tabel KELAS
CREATE TABLE KELAS (
    ID_Kelas INTEGER NOT NULL,
    Lokasi_Kelas VARCHAR2(100),
    Tingkat_Kelas INTEGER,
    Tahun_Ajaran INTEGER,
    GURU_NIP_Guru INTEGER NOT NULL,
    CONSTRAINT KELAS_PK PRIMARY KEY (ID_Kelas),
    CONSTRAINT KELAS_GURU_FK FOREIGN KEY (GURU_NIP_Guru) REFERENCES GURU (NIP_Guru)
);

-- Tabel SISWA
CREATE TABLE SISWA (
    NIS_Siswa INTEGER NOT NULL,
    Nama_Lengkap VARCHAR2(67),
    Tanggal_Lahir DATE,
    Alamat VARCHAR2(67),
    Jenis_Kelamin CHAR(5),
    KELAS_ID_Kelas INTEGER NOT NULL,
    WALI_SISWA_ID_WALISISWA INTEGER NOT NULL,
    CONSTRAINT SISWA_PK PRIMARY KEY (NIS_Siswa),
    CONSTRAINT SISWA_KELAS_FK FOREIGN KEY (KELAS_ID_Kelas) REFERENCES KELAS (ID_Kelas),
    CONSTRAINT SISWA_WALI_SISWA_FK FOREIGN KEY (WALI_SISWA_ID_WALISISWA) REFERENCES WALI_SISWA (ID_WALISISWA)
);

-- Tabel MATA PELAJARAN
CREATE TABLE MATA_PELAJARAN (
    ID_Mapel INTEGER NOT NULL,
    Nama_Mapel VARCHAR2(100),
    Jam_Pelajaran INTEGER,
    Kurikulum VARCHAR2(50),
    CONSTRAINT MATA_PELAJARAN_PK PRIMARY KEY (ID_Mapel)
);

-- Tabel KEHADIRAN
CREATE TABLE KEHADIRAN (
    ID_Kehadiran INTEGER NOT NULL,
    Tanggal DATE,
    Status_Kehadiran VARCHAR2(50),
    Keterangan VARCHAR2(100),
    SISWA_NIS_Siswa INTEGER NOT NULL,
    CONSTRAINT KEHADIRAN_PK PRIMARY KEY (ID_Kehadiran),
    CONSTRAINT KEHADIRAN_SISWA_FK FOREIGN KEY (SISWA_NIS_Siswa) REFERENCES SISWA (NIS_Siswa)
);

-- Tabel NILAI
CREATE TABLE NILAI (
    ID_Nilai INTEGER NOT NULL,
    Nilai INTEGER,
    Semester VARCHAR2(50),
    Tahun_Ajaran VARCHAR2(50),
    SISWA_NIS_Siswa INTEGER NOT NULL,
    MATA_PELAJARAN_ID_Mapel INTEGER NOT NULL,
    CONSTRAINT NILAI_PK PRIMARY KEY (ID_Nilai),
    CONSTRAINT NILAI_SISWA_FK FOREIGN KEY (SISWA_NIS_Siswa) REFERENCES SISWA (NIS_Siswa),
    CONSTRAINT NILAI_MATA_PELAJARAN_FK FOREIGN KEY (MATA_PELAJARAN_ID_Mapel) REFERENCES MATA_PELAJARAN (ID_Mapel)
);

-- Tabel MENGAJAR (Relasi Many-to-Many Guru, Mapel, Kelas)
CREATE TABLE MENGAJAR (
    mengajar_id INTEGER NOT NULL,
    tahun_ajaran INTEGER,
    semester INTEGER,
    GURU_NIP_Guru INTEGER NOT NULL,
    MATA_PELAJARAN_ID_Mapel INTEGER NOT NULL,
    KELAS_ID_Kelas INTEGER NOT NULL,
    CONSTRAINT MENGAJAR_PK PRIMARY KEY (mengajar_id),
    CONSTRAINT MENGAJAR_GURU_FK FOREIGN KEY (GURU_NIP_Guru) REFERENCES GURU (NIP_Guru),
    CONSTRAINT MENGAJAR_MATA_PELAJARAN_FK FOREIGN KEY (MATA_PELAJARAN_ID_Mapel) REFERENCES MATA_PELAJARAN (ID_Mapel),
    CONSTRAINT MENGAJAR_KELAS_FK FOREIGN KEY (KELAS_ID_Kelas) REFERENCES KELAS (ID_Kelas)
);
