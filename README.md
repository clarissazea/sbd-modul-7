# sbd-modul-7

CREATE DATABASE `017_FRD_AnomaliUniverse`;

USE `017_FRD_AnomaliUniverse`;

-- Tabel EntitasAnomali
CREATE TABLE EntitasAnomali (
    id_entitas INT AUTO_INCREMENT PRIMARY KEY,
    nama_entitas VARCHAR(150) NOT NULL UNIQUE,
    tipe_entitas ENUM('Makhluk Hidup', 'Benda Mati', 'Fenomena Abstrak', 'Sound Viral') DEFAULT 'Fenomena Abstrak',
    tingkat_absurditas INT DEFAULT 5,
    tanggal_terdeteksi DATE,
    sumber_origin TEXT
);

-- Tabel KreatorKontenAnomali
CREATE TABLE KreatorKontenAnomali (
    id_kreator INT AUTO_INCREMENT PRIMARY KEY,
    username_kreator VARCHAR(100) NOT NULL UNIQUE,
    platform_utama ENUM('TikTok', 'YouTube', 'Instagram', 'X', 'Lainnya') DEFAULT 'TikTok',
    jumlah_followers BIGINT DEFAULT 0,
    reputasi_brainrot ENUM('Pemula', 'Menengah', 'Ahli', 'Legenda Anomali') DEFAULT 'Pemula'
);


```
- Platform seperti TikTok, YouTube, Instagram, atau X (Twitter) bisa memiliki kreator dengan puluhan juta hingga bahkan miliaran followers. Tipe data INT
- enum: membatasi isi kolom hanya pada daftar nilai tetap yang sudah ditentukan sebelumnya.
```

-- Tabel KontenAnomali
CREATE TABLE KontenAnomali (
    id_konten INT AUTO_INCREMENT PRIMARY KEY,
    id_entitas INT,
    id_kreator INT,
    judul_konten VARCHAR(255) NOT NULL,
    deskripsi_konten TEXT,
    url_konten VARCHAR(512) UNIQUE,
    tipe_media ENUM('Video', 'Audio', 'Gambar', 'Teks') DEFAULT 'Video',
    durasi_detik INT,
    tanggal_unggah DATETIME DEFAULT CURRENT_TIMESTAMP,
    views BIGINT DEFAULT 0,
    likes INT DEFAULT 0,
    shares INT DEFAULT 0,
    potensi_tripping ENUM('Rendah', 'Sedang', 'Tinggi', 'CROCODILO!') DEFAULT 'Sedang',
    
    CONSTRAINT fk_entitas FOREIGN KEY (id_entitas)
        REFERENCES EntitasAnomali(id_entitas)
        ON DELETE SET NULL
        ON UPDATE CASCADE,

    CONSTRAINT fk_kreator FOREIGN KEY (id_kreator)
        REFERENCES KreatorKontenAnomali(id_kreator)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);

```
ON DELETE CASCADE: Jika konten dihapus dari EntitasAnomali, maka semua relasi tag-nya di kontenAnomali juga ikut dihapus.

ON UPDATE CASCADE: Jika id_konten berubah di tabel EntitasAnomali, maka perubahan tersebut juga akan diperbarui di kontenAnomali
```

-- Tabel TagAnomali
CREATE TABLE TagAnomali (
    id_tag INT AUTO_INCREMENT PRIMARY KEY,
    nama_tag VARCHAR(50) NOT NULL UNIQUE
);

-- Tabel KontenTag (many-to-many)
CREATE TABLE KontenTag (
    id_konten INT,
    id_tag INT,
    PRIMARY KEY (id_konten, id_tag),

    CONSTRAINT fk_konten_tag FOREIGN KEY (id_konten)
        REFERENCES KontenAnomali(id_konten)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    CONSTRAINT fk_tag_konten FOREIGN KEY (id_tag)
        REFERENCES TagAnomali(id_tag)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

```
ON DELETE CASCADE: Jika konten dihapus dari KontenAnomali, maka semua relasi tag-nya di KontenTag juga ikut dihapus.

ON UPDATE CASCADE: Jika id_konten berubah di tabel KontenAnomali, maka perubahan tersebut juga akan diperbarui di KontenTag.
```

-- Tabel LogInteraksiBrainrot
CREATE TABLE LogInteraksiBrainrot (
    id_log INT AUTO_INCREMENT PRIMARY KEY,
    id_konten INT,
    username_penonton VARCHAR(100),
    durasi_nonton_detik INT,
    efek_dirasakan TEXT,
    waktu_interaksi DATETIME DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_log_konten FOREIGN KEY (id_konten)
        REFERENCES KontenAnomali(id_konten)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);

ON DELETE CASCADE: Jika konten dihapus dari KontenAnomali, maka semua relasi tag-nya di LogInteraksiBrainrot juga ikut dihapus.

ON UPDATE CASCADE: Jika id_konten berubah di tabel KontenAnomali, maka perubahan tersebut juga akan diperbarui di LogInteraksiBrainrot

-- 1. EntitasAnomali
INSERT INTO EntitasAnomali (nama_entitas, tipe_entitas, tingkat_absurditas, tanggal_terdeteksi, sumber_origin)
VALUES 
('Trippi Troppa Dancer', 'Makhluk Hidup', 8, '2023-11-01', 'Video TikTok India'),
('Bombardini Crocodilo Sound', 'Sound Viral', 9, '2024-01-15', 'Sound effect tak dikenal'),
('Tralalelo Tralala Song', 'Sound Viral', 7, '2023-09-10', 'Lagu anak-anak yang di-remix jadi aneh'),
('Capybara Hydrochaeris', 'Makhluk Hidup', 6, '2022-05-20', 'Berbagai meme capybara masbro'),
('Filter Wajah Menangis Parah', 'Fenomena Abstrak', 7, '2023-06-01', 'Filter Instagram/TikTok'),
('NPC Live Streamer', 'Makhluk Hidup', 9, '2023-08-15', 'Tren live streaming TikTok bertingkah seperti NPC');

-- 2. KreatorKontenAnomali
INSERT INTO KreatorKontenAnomali (Username_Kreator, Platform_Utama, Jumlah_Followers, Reputasi_Brainrot) 
VALUES
('RajaTrippi69', 'TikTok', 1200000, 'Ahli'),
('DJBombardiniOfficial', 'YouTube', 500000, 'Menengah'),
('TralalaQueen', 'TikTok', 750000, 'Menengah'),
('CapybaraEnjoyer_007', 'Instagram', 250000, 'Pemula'),
('LiveNPCMaster', 'TikTok', 2000000, 'Legenda Anomali');


-- 3. KontenAnomali
INSERT INTO KontenAnomali (ID_Entitas, ID_Kreator, Judul_Konten, URL_Konten, Tipe_Media, Durasi_Detik, Views, Likes, Shares, Potensi_Tripping) 
VALUES
(1, 1, 'Trippi Troppa Challenge GONE WILD!', 'tiktok.com/trippi001', 'Video', 30, 5000000, 300000, 150000, 'Tinggi'),
(2, 2, 'BOMBARDINI CROCODILOOO! (10 Hour Loop)', 'youtube.com/bombardini001', 'Audio', 36000, 10000000, 500000, 200000, 'CROCODILO!'),
(3, 3, 'Tralalelo Tralala Remix Full Bass Jedag Jedug', 'tiktok.com/tralala001', 'Audio', 60, 2000000, 150000, 70000, 'Sedang'),
(4, 4, 'Capybara chilling with orange', 'instagram.com/capy001', 'Gambar', NULL, 1000000, 100000, 40000, 'Rendah'),
(6, 5, 'NPC Reacts to Gifts - ICE CREAM SO GOOD', 'tiktok.com/npc001', 'Video', 180, 15000000, 800000, 300000, 'CROCODILO!');


-- 4. TagAnomali
INSERT INTO TagAnomali (Nama_Tag) 
VALUES
('TrippiTroppa'),
('Bombardini'),
('Tralalelo'),
('CapybaraCore'),
('NPCVibes'),
('Absurd'),
('BrainrotLevelMax'),
('HumorGelap');


-- 5. KontenTag (many-to-many)
INSERT INTO KontenTag (ID_Konten, ID_Tag) 
VALUES
(1, 1),
(1, 6),
(1, 7),
(2, 2),
(2, 6),
(2, 7),
(2, 8),
(3, 3),
(3, 6),
(4, 4),
(4, 6),
(5, 5),
(5, 6),
(5, 7);


-- 6. LogInteraksiBrainrot
INSERT INTO LogInteraksiBrainrot (ID_Konten, Username_Penonton, Durasi_Nonton_Detik, Efek_Dirasakan) 
VALUES
(1, 'User123', 25, 'Ikut bergoyang tanpa sadar, merasa sedikit trippy.'),
(2, 'User456', 600, 'Telinga berdenging suara CROCODILO, mulai mempertanyakan realita.'),
(5, 'User789', 170, 'Merasa perlu mengirim gift virtual dan mengulang kata-kata aneh.');




CREATE DATABASE `017_FRD_AnomaliUniverse`;

USE `017_FRD_AnomaliUniverse`;

-- Tabel EntitasAnomali
CREATE TABLE EntitasAnomali (
    id_entitas INT AUTO_INCREMENT PRIMARY KEY,
    nama_entitas VARCHAR(150) NOT NULL UNIQUE,
    tipe_entitas ENUM('Makhluk Hidup', 'Benda Mati', 'Fenomena Abstrak', 'Sound Viral') DEFAULT 'Fenomena Abstrak',
    tingkat_absurditas INT DEFAULT 5,
    tanggal_terdeteksi DATE,
    sumber_origin TEXT
);

-- Tabel KreatorKontenAnomali
CREATE TABLE KreatorKontenAnomali (
    id_kreator INT AUTO_INCREMENT PRIMARY KEY,
    username_kreator VARCHAR(100) NOT NULL UNIQUE,
    platform_utama ENUM('TikTok', 'YouTube', 'Instagram', 'X', 'Lainnya') DEFAULT 'TikTok',
    jumlah_followers BIGINT DEFAULT 0,
    reputasi_brainrot ENUM('Pemula', 'Menengah', 'Ahli', 'Legenda Anomali') DEFAULT 'Pemula'
);

-- Tabel KontenAnomali
CREATE TABLE KontenAnomali (
    id_konten INT AUTO_INCREMENT PRIMARY KEY,
    id_entitas INT,
    id_kreator INT,
    judul_konten VARCHAR(255) NOT NULL,
    deskripsi_konten TEXT,
    url_konten VARCHAR(512) UNIQUE,
    tipe_media ENUM('Video', 'Audio', 'Gambar', 'Teks') DEFAULT 'Video',
    durasi_detik INT,
    tanggal_unggah DATETIME DEFAULT CURRENT_TIMESTAMP,
    views BIGINT DEFAULT 0,
    likes INT DEFAULT 0,
    shares INT DEFAULT 0,
    potensi_tripping ENUM('Rendah', 'Sedang', 'Tinggi', 'CROCODILO!') DEFAULT 'Sedang',
    
    CONSTRAINT fk_entitas FOREIGN KEY (id_entitas)
        REFERENCES EntitasAnomali(id_entitas)
        ON DELETE SET NULL
        ON UPDATE CASCADE,

    CONSTRAINT fk_kreator FOREIGN KEY (id_kreator)
        REFERENCES KreatorKontenAnomali(id_kreator)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);

-- Tabel TagAnomali
CREATE TABLE TagAnomali (
    id_tag INT AUTO_INCREMENT PRIMARY KEY,
    nama_tag VARCHAR(50) NOT NULL UNIQUE
);

-- Tabel KontenTag (many-to-many)
CREATE TABLE KontenTag (
    id_konten INT,
    id_tag INT,
    PRIMARY KEY (id_konten, id_tag),

    CONSTRAINT fk_konten_tag FOREIGN KEY (id_konten)
        REFERENCES KontenAnomali(id_konten)
        ON DELETE CASCADE
        ON UPDATE CASCADE,

    CONSTRAINT fk_tag_konten FOREIGN KEY (id_tag)
        REFERENCES TagAnomali(id_tag)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);

-- Tabel LogInteraksiBrainrot
CREATE TABLE LogInteraksiBrainrot (
    id_log INT AUTO_INCREMENT PRIMARY KEY,
    id_konten INT,
    username_penonton VARCHAR(100),
    durasi_nonton_detik INT,
    efek_dirasakan TEXT,
    waktu_interaksi DATETIME DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT fk_log_konten FOREIGN KEY (id_konten)
        REFERENCES KontenAnomali(id_konten)
        ON DELETE SET NULL
        ON UPDATE CASCADE
);


-- 1. EntitasAnomali
INSERT INTO EntitasAnomali (nama_entitas, tipe_entitas, tingkat_absurditas, tanggal_terdeteksi, sumber_origin)
VALUES 
('Trippi Troppa Dancer', 'Makhluk Hidup', 8, '2023-11-01', 'Video TikTok India'),
('Bombardini Crocodilo Sound', 'Sound Viral', 9, '2024-01-15', 'Sound effect tak dikenal'),
('Tralalelo Tralala Song', 'Sound Viral', 7, '2023-09-10', 'Lagu anak-anak yang di-remix jadi aneh'),
('Capybara Hydrochaeris', 'Makhluk Hidup', 6, '2022-05-20', 'Berbagai meme capybara masbro'),
('Filter Wajah Menangis Parah', 'Fenomena Abstrak', 7, '2023-06-01', 'Filter Instagram/TikTok'),
('NPC Live Streamer', 'Makhluk Hidup', 9, '2023-08-15', 'Tren live streaming TikTok bertingkah seperti NPC');

-- 2. KreatorKontenAnomali
INSERT INTO KreatorKontenAnomali (Username_Kreator, Platform_Utama, Jumlah_Followers, Reputasi_Brainrot) 
VALUES
('RajaTrippi69', 'TikTok', 1200000, 'Ahli'),
('DJBombardiniOfficial', 'YouTube', 500000, 'Menengah'),
('TralalaQueen', 'TikTok', 750000, 'Menengah'),
('CapybaraEnjoyer_007', 'Instagram', 250000, 'Pemula'),
('LiveNPCMaster', 'TikTok', 2000000, 'Legenda Anomali');


-- 3. KontenAnomali
INSERT INTO KontenAnomali (ID_Entitas, ID_Kreator, Judul_Konten, URL_Konten, Tipe_Media, Durasi_Detik, Views, Likes, Shares, Potensi_Tripping) 
VALUES
(1, 1, 'Trippi Troppa Challenge GONE WILD!', 'tiktok.com/trippi001', 'Video', 30, 5000000, 300000, 150000, 'Tinggi'),
(2, 2, 'BOMBARDINI CROCODILOOO! (10 Hour Loop)', 'youtube.com/bombardini001', 'Audio', 36000, 10000000, 500000, 200000, 'CROCODILO!'),
(3, 3, 'Tralalelo Tralala Remix Full Bass Jedag Jedug', 'tiktok.com/tralala001', 'Audio', 60, 2000000, 150000, 70000, 'Sedang'),
(4, 4, 'Capybara chilling with orange', 'instagram.com/capy001', 'Gambar', NULL, 1000000, 100000, 40000, 'Rendah'),
(6, 5, 'NPC Reacts to Gifts - ICE CREAM SO GOOD', 'tiktok.com/npc001', 'Video', 180, 15000000, 800000, 300000, 'CROCODILO!');


-- 4. TagAnomali
INSERT INTO TagAnomali (Nama_Tag) 
VALUES
('TrippiTroppa'),
('Bombardini'),
('Tralalelo'),
('CapybaraCore'),
('NPCVibes'),
('Absurd'),
('BrainrotLevelMax'),
('HumorGelap');


-- 5. KontenTag (many-to-many)
INSERT INTO KontenTag (ID_Konten, ID_Tag) 
VALUES
(1, 1),
(1, 6),
(1, 7),
(2, 2),
(2, 6),
(2, 7),
(2, 8),
(3, 3),
(3, 6),
(4, 4),
(4, 6),
(5, 5),
(5, 6),
(5, 7);


-- 6. LogInteraksiBrainrot
INSERT INTO LogInteraksiBrainrot (ID_Konten, Username_Penonton, Durasi_Nonton_Detik, Efek_Dirasakan) 
VALUES
(1, 'User123', 25, 'Ikut bergoyang tanpa sadar, merasa sedikit trippy.'),
(2, 'User456', 600, 'Telinga berdenging suara CROCODILO, mulai mempertanyakan realita.'),
(5, 'User789', 170, 'Merasa perlu mengirim gift virtual dan mengulang kata-kata aneh.');




DELIMITER $$

CREATE PROCEDURE sp_RegistrasiEntitasBaru (
    IN p_nama_entitas VARCHAR(150),
    IN p_tipe_entitas ENUM('Makhluk Hidup', 'Benda Mati', 'Fenomena Abstrak', 'Sound Viral'),
    IN p_tingkat_absurditas INT,
    IN p_sumber_origin TEXT,
    OUT p_status_registrasi VARCHAR(255)
)
BEGIN
    DECLARE v_exists INT;
    
    SELECT COUNT(*) INTO v_exists
    FROM EntitasAnomali
    WHERE nama_entitas = p_nama_entitas;
    
    IF v_exists > 0 THEN
        SET p_status_registrasi = CONCAT('GAGAL! Entitas ''', p_nama_entitas, ''' sudah terdaftar.');
    ELSE
        INSERT INTO EntitasAnomali (nama_entitas, tipe_entitas, tingkat_absurditas, tanggal_terdeteksi, sumber_origin)
        VALUES (p_nama_entitas, 
                COALESCE(p_tipe_entitas, 'Fenomena Abstrak'), 
                COALESCE(p_tingkat_absurditas, 5), 
                CURRENT_DATE(), 
                p_sumber_origin);
        
        SET p_status_registrasi = CONCAT('SUKSES! Entitas ''', p_nama_entitas, ''' telah ditambahkan.');
    END IF;
END $$

DELIMITER ;


DELIMITER $$

CREATE FUNCTION fn_HitungSkorViralitasKonten (p_id_konten INT)
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    DECLARE v_views BIGINT DEFAULT 0;
    DECLARE v_likes BIGINT DEFAULT 0;
    DECLARE v_shares BIGINT DEFAULT 0;
    DECLARE v_potensi ENUM('Rendah', 'Sedang', 'Tinggi', 'CROCODILO!');
    DECLARE v_score DECIMAL(10,2) DEFAULT 0;

    SELECT views, likes, shares, potensi_tripping INTO v_views, v_likes, v_shares, v_potensi
    FROM KontenAnomali
    WHERE id_konten = p_id_konten;

    SET v_score = (v_views / 10000) + (v_likes * 0.5) + (v_shares * 1);

    IF v_potensi = 'CROCODILO!' THEN
        SET v_score = v_score + 100;
    ELSEIF v_potensi = 'Tinggi' THEN
        SET v_score = v_score + 50;
    ELSEIF v_potensi = 'Sedang' THEN
        SET v_score = v_score + 20;
    END IF;

    RETURN v_score;
END $$

DELIMITER ;


DELIMITER $$

CREATE PROCEDURE sp_GetStatistikEntitasPalingPopuler (
    OUT p_nama_entitas_populer VARCHAR(150),
    OUT p_total_views_entitas BIGINT,
    OUT p_jumlah_konten_terkait INT
)
BEGIN
    DECLARE v_id_entitas INT;

    SELECT id_entitas
    INTO v_id_entitas
    FROM KontenAnomali
    GROUP BY id_entitas
    ORDER BY SUM(views) DESC
    LIMIT 1;

    IF v_id_entitas IS NULL THEN
        SET p_nama_entitas_populer = 'Tidak ada data konten';
        SET p_total_views_entitas = 0;
        SET p_jumlah_konten_terkait = 0;
    ELSE
        SELECT e.nama_entitas, SUM(k.views), COUNT(*)
        INTO p_nama_entitas_populer, p_total_views_entitas, p_jumlah_konten_terkait
        FROM KontenAnomali k
        JOIN EntitasAnomali e ON k.id_entitas = e.id_entitas
        WHERE k.id_entitas = v_id_entitas;
    END IF;
END $$

DELIMITER ;


DELIMITER $$

CREATE PROCEDURE sp_LaporkanKontenBrainrotTeratasBulanan (
    IN p_bulan INT,
    IN p_tahun INT
)
BEGIN
    SELECT 
        judul_konten,
        fn_HitungSkorViralitasKonten(id_konten) AS skor_viralitas,
        tanggal_unggah
    FROM KontenAnomali
    WHERE MONTH(tanggal_unggah) = p_bulan AND YEAR(tanggal_unggah) = p_tahun
    ORDER BY skor_viralitas DESC
    LIMIT 5;
END $$

DELIMITER ;

DELIMITER $$

CREATE FUNCTION fn_KlasifikasiAnomaliOtomatis (p_id_konten INT)
RETURNS VARCHAR(100)
DETERMINISTIC
BEGIN
    DECLARE v_trippi BOOL DEFAULT FALSE;
    DECLARE v_npc BOOL DEFAULT FALSE;
    DECLARE v_total_tags INT DEFAULT 0;
    DECLARE v_result VARCHAR(100);

    IF NOT EXISTS (SELECT 1 FROM KontenAnomali WHERE id_konten = p_id_konten) THEN
        RETURN 'Konten Tidak Ditemukan';
    END IF;

    SELECT COUNT(*) INTO v_trippi
    FROM KontenTag kt
    JOIN TagAnomali t ON kt.id_tag = t.id_tag
    WHERE kt.id_konten = p_id_konten AND t.nama_tag = 'TrippiTroppa';

    SELECT COUNT(*) INTO v_npc
    FROM KontenTag kt
    JOIN TagAnomali t ON kt.id_tag = t.id_tag
    WHERE kt.id_konten = p_id_konten AND t.nama_tag = 'NPCVibes';

    SELECT COUNT(*) INTO v_total_tags
    FROM KontenTag
    WHERE id_konten = p_id_konten;

    IF v_trippi AND v_npc THEN
        SET v_result = 'Kombinasi Trippi NPC';
    ELSEIF v_trippi THEN
        SET v_result = 'Dominan TrippiTroppa';
    ELSEIF v_npc THEN
        SET v_result = 'Dominan NPCVibes';
    ELSEIF v_total_tags > 2 THEN
        SET v_result = 'Campuran Beragam Anomali';
    ELSE
        SET v_result = 'Anomali Standar';
    END IF;

    RETURN v_result;
END $$

DELIMITER ;


-- Panggil prosedur untuk menambahkan entitas baru
CALL sp_RegistrasiEntitasBaru(
    'Anomali Terbang Kuyang',
    'Makhluk Hidup',
    9,
    'Cerita rakyat lokal',
    @status
);

-- Tampilkan hasil status registrasi
SELECT @status;
CALL sp_RegistrasiEntitasBaru(
    'Anomali Terbang Kuyang',
    'Makhluk Hidup',
    9,
    'Cerita rakyat lokal',
    @status
);
SELECT @status;


-- Asumsikan id_konten 1 sudah ada
SELECT fn_HitungSkorViralitasKonten(1) AS skor_viralitas;
SELECT fn_HitungSkorViralitasKonten(99999);


CALL sp_GetStatistikEntitasPalingPopuler(@nama, @views, @jumlah);
SELECT @nama AS nama_entitas, @views AS total_views, @jumlah AS jumlah_konten;


-- Laporan untuk bulan Mei 2025
CALL sp_LaporkanKontenBrainrotTeratasBulanan(5, 2025);



-- Asumsikan id_konten 1 punya tag TrippiTroppa dan NPCVibes
SELECT fn_KlasifikasiAnomaliOtomatis(1) AS klasifikasi;
