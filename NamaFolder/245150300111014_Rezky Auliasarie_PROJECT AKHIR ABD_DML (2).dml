--1. Membuat view untuk menampilkan nilai di bawah 90

CREATE VIEW view_nilai_di_bawah_90 AS
SELECT 
    s.NIS_Siswa,
    s.Nama_Lengkap,
    k.Tingkat_Kelas,
    mp.Nama_Mapel,
    n.Semester,
    n.Tahun_Ajaran,
    n.Nilai
FROM 
    NILAI n
JOIN 
    SISWA s ON s.NIS_Siswa = n.SISWA_NIS_Siswa
JOIN 
    MATA_PELAJARAN mp ON mp.ID_Mapel = n.MATA_PELAJARAN_ID_Mapel
JOIN
    KELAS k ON s.KELAS_ID_Kelas = k.ID_Kelas
WHERE 
    n.Nilai < 90
ORDER BY
    Tingkat_Kelas;


--2. Menampilkan view nilai di bawah 90

SELECT * FROM view_nilai_di_bawah_90;

--3. Membuat view untuk mengetahui wali siswa

CREATE VIEW view_wali_siswa AS
SELECT
    s.NIS_Siswa,
    s.Nama_Lengkap,
    s.alamat,
    k.Tingkat_Kelas,
    w.Nama_wali,
    w.No_Telp_Aktif,
    w.Pekerjaan_wali

FROM
    SISWA s
JOIN
    WALI_SISWA w ON s.WALI_SISWA_ID_WALISISWA = w.ID_WALISISWA
JOIN
    KELAS k ON s.KELAS_ID_Kelas = k.ID_Kelas;

--4. Menampilkan wali siswa 

SELECT * FROM view_wali_siswa;

--5. Membuat viet untuk rata-rata nilai tiap kelas

CREATE VIEW V_RATA_RATA_NILAI_PER_KELAS AS
SELECT 
    K.Tingkat_Kelas,
    K.Tahun_Ajaran,
    M.Semester,
    MP.Nama_Mapel,
    G.Nama_Guru,
    ROUND(AVG(N.Nilai), 2) AS Rata_Rata_Nilai
FROM 
    NILAI N
    JOIN SISWA S ON N.SISWA_NIS_Siswa = S.NIS_Siswa
    JOIN KELAS K ON S.KELAS_ID_Kelas = K.ID_Kelas
    JOIN MATA_PELAJARAN MP ON N.MATA_PELAJARAN_ID_Mapel = MP.ID_Mapel
    JOIN MENGAJAR M ON MP.ID_Mapel = M.MATA_PELAJARAN_ID_Mapel AND K.ID_Kelas = M.KELAS_ID_Kelas
    JOIN GURU G ON M.GURU_NIP_Guru = G.NIP_Guru
GROUP BY 
    K.Tingkat_Kelas,
    K.Tahun_Ajaran,
    M.Semester,
    MP.Nama_Mapel,
    G.Nama_Guru
ORDER BY 
    K.Tingkat_Kelas,
    K.Tahun_Ajaran,
    M.Semester,
    MP.Nama_Mapel,
    G.Nama_Guru; 

--6. Menampilkan view rata-rata nilai tiap kelas

SELECT * FROM  V_RATA_RATA_NILAI_PER_KELAS;