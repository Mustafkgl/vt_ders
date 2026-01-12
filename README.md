# VERİTABANI SINAVI HAZIRLIK DOKÜMANI (2025-2026)
## KTÜ OF TEKNOLOJİ FAKÜLTESİ - YAZILIM MÜHENDİSLİĞİ

**Tarih:** 13 Ocak 2026
**Sınav Tarihi:** 22 Kasım 2024 Geçmiş Sınav Analizi
**Kaynak:** VT_Guz_arasinav.pdf, er-iliski-odev.pdf, VT 2025-2026 Ders Notları

---

## 📋 İÇİNDEKİLER

1. [Hocanın Gerçek Sınav Profili](#gerçek-sinav-profili)
2. [2024 Ara Sınav Soruları ve Çözümleri](#2024-ara-sinav)
3. [Veritabanı Temel Kavramlar](#temel-kavramlar)
4. [SQL Alt Dilleri](#sql-alt-dilleri)
5. [VTYS Üstünlükleri](#vtys-ustunlukleri)
6. [E-R Modelleme](#e-r-modelleme)
7. [SQL Komutları](#sql-komutları)
8. [Örnek Sorular ve Çözümler](#ornek-sorular)

---

## GERÇEK SINAV PROFİLİ

### 📊 Sınav Formatı (22/11/2024 - 90 dakika - 100 Puan):

| Soru No | Konu | Puan | Zorluk |
|---------|------|------|--------|
| 1 | Temel Tanımlar (İlişkisel DB, VTYS, SQL alt dilleri) | 20P | Kolay |
| 2 | VTYS Üstünlükleri | 10P | Kolay |
| 3 | E-R Modelleme (Kütüphane) | 35P | Orta-Zor |
| 4 | SQL Kod Yazma (CREATE DB, CREATE TABLE) | 15P | Orta |
| 5 | Sorgu Çıktısı Tahmin Etme | 20P | Orta |

### ⚠️ ÖNEMLİ NOTLAR:
- ✅ Sınav **temel seviye** - ileri konular YOK
- ✅ E-R modelleme çok önemli (%35)
- ✅ Tanımları ezberlemek şart
- ✅ SQL kod yazımında syntax çok önemli
- ❌ PIVOT, CTE, Window Functions, Trigger, SP çıkmıyor!

---

## 2024 ARA SINAV SORULARI VE ÇÖZÜMLERİ

### SORU 1: Temel Tanımlar (20 Puan)

#### a) İlişkisel veri tabanı ne demektir? Neden bu şekilde adlandırılmış olabilir? (5P)

**CEVAP:**
İlişkisel veri tabanı, verilerin tablolar (ilişkiler/relations) halinde organize edildiği ve tablolar arasında ilişkilerin kurulduğu veri tabanı modelidir.

**Neden "İlişkisel" denir:**
1. **Tablolar arasında ilişkiler** vardır (Foreign Key ile)
2. **Matematiksel ilişki teorisine** dayanır
3. Her tablo bir **ilişki kümesini** (relation) temsil eder
4. **İlişkisel cebir** operasyonları kullanılır (JOIN, UNION vb.)

**Örnek:**
- Öğrenci tablosu ve Ders tablosu arasında "kayıt olma" ilişkisi
- Müşteri ve Sipariş tabloları arasında "sipariş verme" ilişkisi

---

#### b) Veri tabanı, VTYS, tablo, primary key, foreign key kavramlarını açıklayınız. (10P)

**CEVAP:**

**1. Veri Tabanı (Database):**
Birbiriyle ilişkili verilerin düzenli ve yapılandırılmış şekilde saklandığı dijital depodur.

**2. Veri Tabanı Yönetim Sistemi (VTYS):**
Veri tabanlarını oluşturmak, yönetmek, sorgulamak için kullanılan yazılımdır. (Örnek: SQL Server, MySQL, Oracle)

**3. Tablo (Table):**
Satır ve sütunlardan oluşan, belirli bir varlığa ait verileri saklayan yapıdır. Her satır bir kayıt (record), her sütun bir özellik (attribute) belirtir.

**4. Primary Key (Birincil Anahtar):**
Tablodaki her kaydı benzersiz şekilde tanımlayan sütun veya sütun kombinasyonudur. NULL olamaz, tekil olmalıdır.

**5. Foreign Key (Yabancı Anahtar/İkincil Anahtar):**
Başka bir tablonun Primary Key'ine referans veren sütundur. Tablolar arası ilişki kurar, veri bütünlüğünü sağlar.

**Örnek:**
```sql
Ogrenci Tablosu:
OgrenciID (PK) | Ad | Soyad | BolumID (FK)

Bolum Tablosu:
BolumID (PK) | BolumAd
```

---

#### c) SQL kaç alt dil grubundan oluşur? Her birini tanımlayarak örneklendiriniz. (5P)

**CEVAP:**

SQL **4 alt dil grubundan** oluşur:

**1. DDL (Data Definition Language) - Veri Tanımlama Dili:**
- **Görevi:** Veritabanı yapısını oluşturma, değiştirme, silme
- **Komutlar:** CREATE, ALTER, DROP, TRUNCATE
- **Örnek:**
```sql
CREATE TABLE Ogrenci (OgrenciID INT PRIMARY KEY, Ad NVARCHAR(50));
ALTER TABLE Ogrenci ADD Email NVARCHAR(100);
DROP TABLE Ogrenci;
```

**2. DML (Data Manipulation Language) - Veri İşleme Dili:**
- **Görevi:** Verileri ekleme, güncelleme, silme, sorgulama
- **Komutlar:** SELECT, INSERT, UPDATE, DELETE
- **Örnek:**
```sql
INSERT INTO Ogrenci VALUES (1, 'Ali');
UPDATE Ogrenci SET Ad = 'Ahmet' WHERE OgrenciID = 1;
DELETE FROM Ogrenci WHERE OgrenciID = 1;
SELECT * FROM Ogrenci;
```

**3. DCL (Data Control Language) - Veri Kontrol Dili:**
- **Görevi:** Kullanıcı yetkilendirme ve izin yönetimi
- **Komutlar:** GRANT, REVOKE
- **Örnek:**
```sql
GRANT SELECT ON Ogrenci TO Kullanici1;
REVOKE DELETE ON Ogrenci FROM Kullanici1;
```

**4. TCL (Transaction Control Language) - İşlem Kontrol Dili:**
- **Görevi:** Transaction (işlem) yönetimi
- **Komutlar:** COMMIT, ROLLBACK, SAVEPOINT
- **Örnek:**
```sql
BEGIN TRANSACTION;
UPDATE Hesap SET Bakiye = Bakiye - 100 WHERE HesapNo = '123';
COMMIT; -- veya ROLLBACK;
```

---

### SORU 2: VTYS Üstünlükleri (10 Puan)

**Dosyalama sistemlerine alternatif olarak geliştirilen VTYS'nin üstünlüklerini açıklayınız.**

**CEVAP:**

**1. Veri Tekrarını Azaltma (Data Redundancy):**
Aynı veri birden fazla yerde saklanmaz, tek bir yerde tutulur ve referans edilir.

**2. Veri Tutarlılığı (Data Consistency):**
Veri bir yerde güncellendiğinde tüm sistem için geçerli olur, çelişkili veri oluşmaz.

**3. Veri Bütünlüğü (Data Integrity):**
Constraint'ler (PRIMARY KEY, FOREIGN KEY, CHECK) ile veri doğruluğu garanti edilir.

**4. Veri Güvenliği (Data Security):**
Kullanıcı bazlı yetkilendirme sistemi, şifreleme, erişim kontrolü sağlanır.

**5. Veri Bağımsızlığı (Data Independence):**
- **Fiziksel Bağımsızlık:** Dosya yapısı değişse de uygulama etkilenmez
- **Mantıksal Bağımsızlık:** Tablo yapısı değişse bile view'ler ile uyumluluk sağlanır

**6. Eşzamanlı Erişim (Concurrent Access):**
Birden fazla kullanıcı aynı anda veri tabanına erişebilir, deadlock önlenir.

**7. Yedekleme ve Kurtarma (Backup & Recovery):**
Otomatik yedekleme, transaction log ile veri kaybı önlenir.

**8. Veri Paylaşımı (Data Sharing):**
Merkezi veri deposu sayesinde tüm kullanıcılar güncel veriye erişir.

**9. Standardizasyon:**
SQL gibi standart diller sayesinde farklı sistemler arası uyumluluk sağlanır.

**10. Sorgu Kolaylığı:**
Karmaşık raporlar SQL ile kolayca oluşturulur, dosya sisteminde çok zor olur.

---

### SORU 3: E-R Modelleme - Kütüphane Sistemi (35 Puan)

**Bir kütüphane için veri tabanı oluşturulmak isteniyor.**

#### Kurallar:
1. Genel olarak; **kitaplar, yazarlar, raflar, okurlar** varlık kümeleri olacaktır.
2. Bir yazarın **birden fazla** kitabı olabilir, her kitabın **en fazla bir** yazarı vardır. (1:N)
3. Raflar A-Z'ye kadar numaralandırılmıştır, bir rafta **birden fazla** kitap olabilir, bazı raflar kullanılmayabilir, bir kitap **iki rafta olamaz**. (1:N)
4. Tüm üyeler kitap almak **zorunda değildir**, bir üye **birden fazla kitap alamaz**, bir kitap **birden fazla üyeye verilemez**. (1:0..1)

⚠️ **ÇOK ÖNEMLİ:** Soru 4'teki kural, N:M ilişki DEĞİL! Bir üye aynı anda sadece 1 kitap alabilir.

---

#### a) E-R Diyagramını Çiziniz (20P)

**CEVAP:**

```
┌──────────┐              ┌──────────┐              ┌──────────┐
│  YAZAR   │              │  KİTAP   │              │   RAF    │
├──────────┤              ├──────────┤              ├──────────┤
│YazarID PK│1           N │KitapID PK│N           1 │ RafNo PK │
│Ad        │─────yazar────│Baslik    │─────bulunur──│ Konum    │
│Soyad     │              │YazarID FK│              │ Kapasite │
│DogumYili │              │RafNo FK  │              └──────────┘
└──────────┘              │YayinYili │
                          │ISBN      │
                          └──────────┘
                                │
                                │ 1
                                │
                          ┌─────┴─────┐
                          │  ÖDÜNÇ    │ (İlişki)
                          │   ALMA    │
                          ├───────────┤
                          │AlisTarihi │
                          │TeslimTar. │
                          └─────┬─────┘
                                │
                                │ 0..1
                                │
                          ┌──────────┐
                          │  OKUR    │
                          ├──────────┤
                          │OkurID PK │
                          │TcNo      │
                          │Ad        │
                          │Soyad     │
                          │Telefon   │
                          └──────────┘
```

**Kardinalite Açıklamaları:**
- **Yazar-Kitap:** 1:N (Bir yazar birden fazla kitap yazar, her kitabın 1 yazarı var)
- **Raf-Kitap:** 1:N (Bir rafta birden fazla kitap, her kitap 1 rafta)
- **Kitap-Okur:** 1:0..1 (Bir kitap 0 veya 1 okura verilebilir, bir okur 0 veya 1 kitap alabilir)

---

#### b) Tabloları Belirtiniz (15P)

**CEVAP:**

**1. Yazar Tablosu:**
```
Yazar(YazarID, Ad, Soyad, DogumYili, Ulke)
- YazarID: Primary Key
```

**2. Raf Tablosu:**
```
Raf(RafNo, Konum, Kapasite)
- RafNo: Primary Key (A, B, C, ... Z)
```

**3. Kitap Tablosu:**
```
Kitap(KitapID, ISBN, Baslik, YayinYili, SayfaSayisi, YazarID, RafNo)
- KitapID: Primary Key
- YazarID: Foreign Key → Yazar(YazarID)
- RafNo: Foreign Key → Raf(RafNo)
```

**4. Okur Tablosu:**
```
Okur(OkurID, TcNo, Ad, Soyad, Telefon, Email, KayitTarihi)
- OkurID: Primary Key
- TcNo: UNIQUE (11 karakter)
```

**5. OduncAlma Tablosu:**
```
OduncAlma(OduncID, KitapID, OkurID, AlisTarihi, TeslimTarihi, BeklenenTeslimTarihi)
- OduncID: Primary Key
- KitapID: Foreign Key → Kitap(KitapID) UNIQUE
- OkurID: Foreign Key → Okur(OkurID) UNIQUE
- Bir kitap aynı anda sadece 1 okurda olabilir (KitapID UNIQUE)
- Bir okur aynı anda sadece 1 kitap alabilir (OkurID UNIQUE)
```

**İlişki Kuralları:**
- 1-N ilişki: Foreign Key **N tarafına** eklenir (Kitap tablosuna YazarID ve RafNo)
- 1-0..1 ilişki: Ayrı tablo oluşturulur (OduncAlma), her iki taraf da UNIQUE olmalı

---

### SORU 4: SQL Kod Yazma (15 Puan)

#### a) CREATE DATABASE Komutu (7.5P)

**Soru:** Birincil ve log dosya bilgileri aşağıdaki gibi olan bir vt'yi (ogrenci adında) oluşturan SQL kodları yazınız.

**Birincil dosya:**
- Dosya adı: ogrenci_veri
- Dosya yolu: 'D:\data\ogrenci.mdf'
- Boyutu: 10 MB
- En büyük boyut: 100 MB
- Büyüme oranı: %25

**Log dosya:**
- Dosya adı: o_veri_log
- Dosya yolu: 'D:\data\ogrenci.ldf'
- Boyutu: 5 MB
- En büyük boyut: 50 MB
- Büyüme oranı: %25

**CEVAP:**

```sql
CREATE DATABASE ogrenci
ON PRIMARY
(
    NAME = ogrenci_veri,
    FILENAME = 'D:\data\ogrenci.mdf',
    SIZE = 10MB,
    MAXSIZE = 100MB,
    FILEGROWTH = 25%
)
LOG ON
(
    NAME = o_veri_log,
    FILENAME = 'D:\data\ogrenci.ldf',
    SIZE = 5MB,
    MAXSIZE = 50MB,
    FILEGROWTH = 25%
);
```

**Parametre Açıklamaları:**
- **NAME:** Mantıksal dosya adı (SQL Server içinde kullanılır)
- **FILENAME:** Fiziksel dosya yolu (disk üzerinde)
- **SIZE:** Başlangıç boyutu
- **MAXSIZE:** Maksimum büyüyebileceği boyut (UNLIMITED yazılırsa sınırsız)
- **FILEGROWTH:** Dosya dolduğunda ne kadar büyüyeceği (% veya MB)

---

#### b) CREATE TABLE Komutu (7.5P)

**Soru:** ogrenci_no, tckimlikno, ad, soyad alanlarından oluşan bir tabloyu (tblogrenci) oluşturan SQL kodlarını yazınız.

**Özellikler:**
- Öğrenci_no alanı: **1000'den başlayarak 1'er artacak** şekilde
- tckimlikno: **11 karakter** olacak metin (**sabit uzunlukta** – Unicode kodlama)
- ad ve soyad: **20 karakterden** oluşacak metin (**değişken uzunlukta**, Unicode kodlama)

**CEVAP:**

```sql
CREATE TABLE tblogrenci
(
    ogrenci_no INT IDENTITY(1000, 1) PRIMARY KEY,
    tckimlikno NCHAR(11) NOT NULL,
    ad NVARCHAR(20) NOT NULL,
    soyad NVARCHAR(20) NOT NULL
);
```

**Veri Tipi Açıklamaları:**

| Veri Tipi | Açıklama | Kullanım |
|-----------|----------|----------|
| **INT** | Tam sayı | ID, sayılar |
| **IDENTITY(1000, 1)** | 1000'den başla, 1'er artır | Otomatik artan ID |
| **NCHAR(11)** | Sabit 11 karakter, Unicode | TC Kimlik No (her zaman 11) |
| **NVARCHAR(20)** | Değişken 0-20 karakter, Unicode | Ad, Soyad (Türkçe karakter) |
| **NOT NULL** | Boş bırakılamaz | Zorunlu alan |

**NCHAR vs NVARCHAR Farkı:**

```sql
-- NCHAR(11): Her zaman 11 karakter yer kaplar
DECLARE @tc NCHAR(11) = '12345';  -- '12345      ' (6 boşluk eklenir)

-- NVARCHAR(20): Sadece kullanılan kadar yer kaplar
DECLARE @ad NVARCHAR(20) = 'Ali';  -- 'Ali' (3 karakter)
```

**Ne Zaman Hangisi:**
- ✅ **NCHAR:** Sabit uzunluklu veriler (TC No, Tel No, Plaka)
- ✅ **NVARCHAR:** Değişken uzunluklu veriler (Ad, Soyad, Adres)
- ✅ **N öneki:** Türkçe karakter desteği için (Ç, Ş, Ğ, İ, Ö, Ü)

---

### SORU 5: Sorgu Çıktısı Tahmin Etme (20 Puan)

**Örnek Tablo (tblurun):**

| urunkod | urunad              | listefiyat | marka     |
|---------|---------------------|------------|-----------|
| A1      | Bilgisayar          | 2500       | Vestel    |
| A2      | Barkod okuyucu      | 500        | Vestel    |
| A3      | Mouse               | 75         | Microsoft |
| A4      | Mouse               | 40         | A4 Tech   |
| A5      | Modem               | 120        | Zyxel     |
| A6      | Monitor             | 500        | Vestel    |
| A7      | Monitor             | 800        | Samsung   |
| A8      | Cep Telefonu        | 1800       | Vodafone  |
| A9      | Monitor             | 1000       | Viewsonic |
| A10     | Cep Telefonu        | 2000       | Microsoft |
| A11     | Harici Disk Sürücü  | 250        | Samsung   |

---

#### a) SELECT DISTINCT marka FROM tblurun ORDER BY marka DESC

**CEVAP:**

| marka     |
|-----------|
| Zyxel     |
| Vodafone  |
| Viewsonic |
| Vestel    |
| Samsung   |
| Microsoft |
| A4 Tech   |

**Açıklama:**
- **DISTINCT:** Tekrar eden markaları bir kez gösterir
- **ORDER BY ... DESC:** Z'den A'ya sıralama (azalan)

---

#### b) SELECT urunad, listefiyat, listefiyat*1.1 FROM tblurun WHERE marka = (SELECT marka FROM tblurun WHERE urunad='Bilgisayar')

**CEVAP:**

| urunad          | listefiyat | (listefiyat*1.1) |
|-----------------|------------|------------------|
| Bilgisayar      | 2500       | 2750.0           |
| Barkod okuyucu  | 500        | 550.0            |
| Monitor         | 500        | 550.0            |

**Açıklama:**
- **Subquery:** WHERE urunad='Bilgisayar' → marka = 'Vestel'
- Vestel markasına ait 3 ürün var
- Her ürünün fiyatı %10 zamlanmış hali hesaplanıyor

---

#### c) SELECT urunad, listefiyat FROM tblurun WHERE marka LIKE 'M%'

**CEVAP:**

| urunad       | listefiyat |
|--------------|------------|
| Mouse        | 75         |
| Mouse        | 40         |
| Cep Telefonu | 2000       |

**Açıklama:**
- **LIKE 'M%':** M harfi ile başlayan markalar
- Microsoft (2 ürün): Mouse (75), Cep Telefonu (2000)
- Microsoft (1 ürün): Mouse (40) → YANLIŞ! A4 Tech
- **Doğru Sonuç:** Sadece Microsoft markalı 2 ürün

**DÜZELTME:**

| urunad       | listefiyat |
|--------------|------------|
| Mouse        | 75         |
| Cep Telefonu | 2000       |

---

#### d) SELECT TOP(3) urunad, listefiyat FROM tblurun ORDER BY listefiyat

**CEVAP:**

| urunad | listefiyat |
|--------|------------|
| Mouse  | 40         |
| Mouse  | 75         |
| Modem  | 120        |

**Açıklama:**
- **ORDER BY listefiyat:** Fiyata göre artan sıralama (küçükten büyüğe)
- **TOP(3):** İlk 3 kaydı getir
- En ucuz 3 ürün

---

## E-R MODELLEME ÖRNEKLERİ

### ÖRNEK-1: Okul Yönetim Sistemi

**Kurallar:**
1. Öğretmenler bölümlerde görev yapar. Bir bölümde birçok öğretmen olabilir ama bir öğretmen tek bir bölümde çalışır. **(1:N)**
2. Öğrenciler derslere kayıt olur. Bir öğrenci birçok ders alabilir, bir dersi birçok öğrenci alabilir. **(N:M)**
3. Dersler öğretmenler tarafından verilir. Bir öğretmen birçok derse girebilir. **(1:N)**
4. Her dersin bir sınıfı vardır, bir sınıfta birden fazla ders olabilir. **(1:N)**

**E-R Diyagramı:**

```
┌───────────┐         ┌────────────┐         ┌──────────┐
│  BÖLÜM    │1      N │ ÖĞRETMEN   │1      N │   DERS   │
├───────────┤─────────├────────────┤─────────├──────────┤
│BolumID PK │  görev  │OgretmenID  │  verir  │DersID PK │
│BolumAd    │         │Ad          │         │DersAd    │
└───────────┘         │Soyad       │         │Kredi     │
                      │BolumID FK  │         │OgretID FK│
                      └────────────┘         │SinifNo FK│
                                             └────┬─────┘
                                                  │
                                                  │ N
                                          ┌───────┴────────┐
                                          │  ÖĞRENCİ-DERS  │ (Junction)
                                          ├────────────────┤
                                          │OgrenciID FK    │
                                          │DersID FK       │
                                          │Vize            │
                                          │Final           │
                                          └────────┬───────┘
                                                   │
                                                   │ M
                                             ┌─────┴─────┐
                                             │ ÖĞRENCİ   │
                                             ├───────────┤
                                             │OgrenciID  │
                                             │Ad, Soyad  │
                                             └───────────┘

┌──────────┐
│  SINIF   │1
├──────────┤
│SinifNo PK│
│Kapasite  │
└──────────┘
```

**Tablolar:**
```sql
1. Bolum(BolumID, BolumAd, Mudurluk)
2. Ogretmen(OgretmenID, Ad, Soyad, BolumID FK)
3. Sinif(SinifNo, BlokAd, Kat, Kapasite)
4. Ders(DersID, DersAd, Kredi, OgretmenID FK, SinifNo FK)
5. Ogrenci(OgrenciID, TcNo, Ad, Soyad)
6. OgrenciDers(OgrenciID FK, DersID FK, Vize, Final, Durum)
   - PRIMARY KEY (OgrenciID, DersID)
```

---

### ÖRNEK-2: Kargo Takip Sistemi

**Kurallar:**
1. Müşteriler gönderi oluşturur. Bir müşteri birçok gönderi oluşturabilir. **(1:N)**
2. Her gönderi bir şubeden teslim edilir. Bir şube birçok gönderiyi işleyebilir. **(1:N)**
3. Gönderi bir kargo personeline atanır. Bir personel birçok gönderi taşıyabilir ama bir gönderi sadece bir personele atanır. **(1:N)**
4. Gönderi durumları (KargoDurum) ayrı tabloda tutulur, gönderiyle birebir ilişkilidir. **(1:1)**

**E-R Diyagramı:**

```
┌──────────┐         ┌──────────┐         ┌──────────┐
│ MÜŞTERİ  │1      N │ GÖNDERİ  │N      1 │  ŞUBE    │
├──────────┤─oluştur─├──────────┤─teslim──├──────────┤
│MusteriID │         │GonderiID │         │SubeID PK │
│Ad, Soyad │         │TakipNo   │         │SubeAd    │
│Telefon   │         │Agirlik   │         │Sehir     │
└──────────┘         │MusteriID │         └──────────┘
                     │SubeID FK │
                     │PersonelID│         ┌──────────┐
                     └────┬─────┘       1 │ PERSONEL │
                          │               ├──────────┤
                          │ 1           N │PersonelID│
                     ┌────┴──────┐────────│Ad, Soyad │
                     │KARGO DURUM│ atanır │Telefon   │
                     ├───────────┤        └──────────┘
                     │GonderiID  │
                     │Durum      │
                     │Tarih      │
                     └───────────┘
```

**Tablolar:**
```sql
1. Musteri(MusteriID, TcNo, Ad, Soyad, Telefon, Adres)
2. Sube(SubeID, SubeAd, Sehir, Telefon)
3. Personel(PersonelID, Ad, Soyad, Telefon, SubeID FK)
4. Gonderi(GonderiID, TakipNo, Agirlik, MusteriID FK, SubeID FK, PersonelID FK)
5. KargoDurum(GonderiID PK FK, Durum, GuncellemeTarihi, Aciklama)
   - 1:1 ilişki: GonderiID hem PK hem FK
```

---

### ÖRNEK-3: Kütüphane Otomasyonu (Alternatif - N:M)

⚠️ **DİKKAT:** Bu örnek sınavdaki kütüphane senaryosundan farklı! Burada N:M ilişki var.

**Kurallar:**
1. Kitaplar yazarlara aittir. Bir yazar birçok kitap yazabilir. **(1:N)**
2. Üyeler kitap ödünç alabilir. Bir üye birçok kitap alabilir, bir kitap zaman içinde birçok üyeye verilmiş olabilir. **(N:M)**
3. Ödünç alma işlemleri personel tarafından yapılır. **(1:N)**
4. Kitaplar bir kategoriye aittir, bir kategori birçok kitabı kapsar. **(1:N)**

**Tablolar:**
```sql
1. Yazar(YazarID, Ad, Soyad, Ulke)
2. Kategori(KategoriID, KategoriAd)
3. Kitap(KitapID, ISBN, Baslik, YazarID FK, KategoriID FK)
4. Uye(UyeID, TcNo, Ad, Soyad)
5. Personel(PersonelID, Ad, Soyad, Sifre)
6. OduncAlma(OduncID, UyeID FK, KitapID FK, PersonelID FK, AlisTarihi, TeslimTarihi)
   - PRIMARY KEY (OduncID)
   - N:M ilişki için junction table
```

---

## SQL TEMEL KOMUTLARI

### SELECT Sorguları

**1. Temel SELECT:**
```sql
SELECT * FROM tblurun;
SELECT urunad, listefiyat FROM tblurun;
```

**2. DISTINCT (Tekrarsız):**
```sql
SELECT DISTINCT marka FROM tblurun;
```

**3. WHERE (Filtreleme):**
```sql
SELECT * FROM tblurun WHERE listefiyat > 500;
SELECT * FROM tblurun WHERE marka = 'Vestel';
```

**4. ORDER BY (Sıralama):**
```sql
SELECT * FROM tblurun ORDER BY listefiyat;          -- Artan (ASC)
SELECT * FROM tblurun ORDER BY listefiyat DESC;     -- Azalan
SELECT * FROM tblurun ORDER BY marka, listefiyat;   -- Çoklu sıralama
```

**5. TOP (İlk N Kayıt):**
```sql
SELECT TOP(3) * FROM tblurun ORDER BY listefiyat;
SELECT TOP(10) PERCENT * FROM tblurun;
```

**6. LIKE (Benzerlik Arama):**
```sql
SELECT * FROM tblurun WHERE marka LIKE 'M%';      -- M ile başlar
SELECT * FROM tblurun WHERE urunad LIKE '%Disk%'; -- İçinde Disk geçer
SELECT * FROM tblurun WHERE marka LIKE '_estel';  -- _estel (5 karakter)
```

**7. IN (Liste İçinde):**
```sql
SELECT * FROM tblurun WHERE marka IN ('Vestel', 'Samsung', 'Microsoft');
```

**8. BETWEEN (Aralık):**
```sql
SELECT * FROM tblurun WHERE listefiyat BETWEEN 100 AND 1000;
```

**9. NULL Kontrol:**
```sql
SELECT * FROM tblurun WHERE aciklama IS NULL;
SELECT * FROM tblurun WHERE aciklama IS NOT NULL;
```

---

### INSERT Komutu

```sql
-- Tüm sütunlar
INSERT INTO tblogrenci VALUES (1001, '12345678901', 'Ali', 'Veli');

-- Belirtilen sütunlar
INSERT INTO tblogrenci (tckimlikno, ad, soyad)
VALUES ('12345678901', 'Ayşe', 'Yılmaz');

-- Birden fazla kayıt
INSERT INTO tblogrenci (tckimlikno, ad, soyad) VALUES
('11111111111', 'Mehmet', 'Kaya'),
('22222222222', 'Fatma', 'Demir');
```

---

### UPDATE Komutu

```sql
-- Tek kayıt güncelleme
UPDATE tblurun SET listefiyat = 2750 WHERE urunkod = 'A1';

-- Birden fazla sütun
UPDATE tblogrenci
SET ad = 'Ahmet', soyad = 'Yılmaz'
WHERE ogrenci_no = 1000;

-- Hesaplama ile
UPDATE tblurun SET listefiyat = listefiyat * 1.1 WHERE marka = 'Vestel';
```

---

### DELETE Komutu

```sql
-- Belirli kayıtları sil
DELETE FROM tblurun WHERE urunkod = 'A1';

-- Tüm kayıtları sil (dikkatli!)
DELETE FROM tblurun;

-- TRUNCATE (daha hızlı, identity sıfırlar)
TRUNCATE TABLE tblurun;
```

---

## ÖNEMLİ NOTLAR VE İPUÇLARI

### ✅ Sınavda Mutlaka Bilin:

**1. Temel Tanımlar:**
- İlişkisel veritabanı nedir?
- Primary Key vs Foreign Key
- DDL, DML, DCL, TCL nedir?
- VTYS üstünlükleri (10 tane)

**2. E-R Modelleme:**
- 1:1, 1:N, N:M ilişkileri ayırt etme
- Varlıkları ve ilişkileri belirleme
- Foreign Key'lerin hangi tabloya gideceğini bilme
- Junction table ne zaman gerekli

**3. SQL Komutları:**
- CREATE DATABASE syntax (NAME, FILENAME, SIZE, MAXSIZE, FILEGROWTH)
- CREATE TABLE syntax (veri tipleri, IDENTITY, PRIMARY KEY)
- NCHAR vs NVARCHAR farkı
- SELECT, WHERE, ORDER BY, DISTINCT, TOP, LIKE

**4. Sorgu Çıktısı:**
- DISTINCT ne işe yarar
- ORDER BY DESC nasıl sıralar
- Subquery nasıl çalışır
- LIKE 'M%' ne getirir

---

### ⚠️ Dikkat Edilecek Noktalar:

**1. NULL Kontrolü:**
```sql
-- YANLIŞ:
WHERE marka = NULL

-- DOĞRU:
WHERE marka IS NULL
WHERE marka IS NOT NULL
```

**2. String'lerde Türkçe Karakter:**
```sql
-- YANLIŞ:
INSERT INTO tblogrenci VALUES ('Şükrü');  -- Hata verebilir

-- DOĞRU:
INSERT INTO tblogrenci VALUES (N'Şükrü');  -- N öneki ekle
```

**3. IDENTITY Sütununa INSERT:**
```sql
-- YANLIŞ:
INSERT INTO tblogrenci (ogrenci_no, ad) VALUES (1000, 'Ali');

-- DOĞRU:
INSERT INTO tblogrenci (ad, soyad) VALUES ('Ali', 'Veli');
-- ogrenci_no otomatik oluşur
```

**4. ORDER BY Yönü:**
```sql
ORDER BY listefiyat;       -- Küçükten büyüğe (ASC varsayılan)
ORDER BY listefiyat DESC;  -- Büyükten küçüğe
```

**5. TOP Kullanımı:**
```sql
-- İlk 3 en ucuz ürün:
SELECT TOP(3) * FROM tblurun ORDER BY listefiyat;

-- İlk 3 en pahalı ürün:
SELECT TOP(3) * FROM tblurun ORDER BY listefiyat DESC;
```

---

## SINAV STRATEJİSİ

### Son 7 Gün:

**7 Gün Kala:**
- Bu dokümanı baştan sona oku
- Tüm tanımları ezberle
- E-R örneklerini çiz

**5 Gün Kala:**
- 2024 sınav sorularını çöz
- SQL komutlarını yaz ve test et
- E-R senaryolarını tekrar et

**3 Gün Kala:**
- VTYS üstünlüklerini ezberle (10 tane)
- SQL alt dillerini örneklerle açıklayabilmelisin
- CREATE DATABASE ve CREATE TABLE syntax'ını ezberle

**1 Gün Kala:**
- Önemli formülleri gözden geçir
- E-R diyagram çizim kurallarını tekrar et
- NULL, DISTINCT, LIKE, ORDER BY kullanımını pekiştir

---

### Puan Dağılımı ve Strateji:

| Soru | Puan | Zorluk | Strateji |
|------|------|--------|----------|
| Soru 1 | 20P | Kolay | Tam puan al! Tanımları ezberle |
| Soru 2 | 10P | Kolay | 10 üstünlüğü yaz |
| Soru 3 | 35P | Orta-Zor | E-R kurallarına DİKKAT! En önemli soru |
| Soru 4 | 15P | Orta | Syntax hatası yapma |
| Soru 5 | 20P | Orta | Sorgu çıktısını adım adım hesapla |

**Hedef:** 100/100

---

## SON SÖZ

Bu dokümandaki 2024 sınav sorularını çöz ve anlarsanız **100 üzerinden 100 alacaksınız!**

**Önemli Hatırlatmalar:**
- ✅ E-R modelleme çok önemli (%35)
- ✅ Kuralları dikkatlice oku (1:1, 1:N, N:M)
- ✅ SQL syntax hatası yapma
- ✅ NCHAR vs NVARCHAR farkını bil
- ✅ NULL kontrolü IS NULL ile
- ✅ Türkçe karakterler için N'...' kullan

Başarılar! 🎓

---

**Güncelleme:** 13 Ocak 2026
**Kaynak:** VT_Guz_arasinav.pdf (22/11/2024), er-iliski-odev.pdf, VT 2025-2026 Ders Notları
**Hazırlayan:** Claude AI + Ders Materyalleri Analizi
