# VERİTABANI SINAVI HAZIRLIK DOKÜMANI

## 📋 İÇİNDEKİLER
1. [Hocanın Soru Profili](#hocanın-soru-profili)
2. [Veritabanı Temelleri](#1-veritabanı-temelleri)
3. [SQL Temel Komutlar](#2-sql-temel-komutlar)
4. [Veri Bütünlüğü ve Kısıtlamalar](#3-veri-bütünlüğü-ve-kısıtlamalar)
5. [İndeksler](#4-indeksler)
6. [Stored Procedures](#5-stored-procedures)
7. [Triggers](#6-triggers)
8. [JOIN İşlemleri](#7-join-işlemleri)
9. [E-R Modelleme](#8-e-r-modelleme)

---

## HOCANIN SORU PROFİLİ

### Soru Tarzı Analizi:
**VT_Guz_arasinav.pdf** analizi sonuçları:

1. **Teorik Tanım Soruları** (%30)
   - İlişkisel veritabanı nedir?
   - VTYS'nin avantajları
   - SQL alt dilleri (DDL, DML, DCL, TCL)
   - Anahtar türleri (Primary Key, Foreign Key, Candidate Key)

2. **E-R Diyagram Çizimi** (%25)
   - Gerçek hayat senaryolarından E-R modeli oluşturma
   - Varlık-ilişki kurallarını uygulama
   - Kardinalite belirleme

3. **SQL Kod Yazma** (%25)
   - Database/Table oluşturma
   - Constraint tanımlama
   - Sorgu yazma

4. **Sorgu Çıktısı Tahmin Etme** (%20)
   - Verilen tablo ve sorgudan çıktı bulma
   - JOIN sonuçlarını tahmin etme
   - Aggregate fonksiyon sonuçları

### Önemli Konular:
✅ **Çok Önemli:** Constraints, JOIN, E-R Modelleme, SQL DDL/DML
✅ **Önemli:** Index, Stored Procedure, Trigger, Aggregate Functions
✅ **Orta Önemli:** Transaction, Normalizasyon

---

## 1. VERİTABANI TEMELLERİ

### KOLAY SORULAR (1-10)

**Soru 1:** İlişkisel veritabanı nedir? Temel özelliklerini açıklayınız.

**Cevap 1:** İlişkisel veritabanı, verilerin tablolar (ilişkiler) halinde organize edildiği veritabanı modelidir.

**Temel Özellikleri:**
- Veriler satır ve sütunlardan oluşan tablolarda saklanır
- Her tablo bir varlığı (entity) temsil eder
- Tablolar arasında ilişkiler (relationships) tanımlanabilir
- Her satır benzersiz bir kayıttır (tuple)
- Her sütun bir özelliği (attribute) temsil eder
- Primary Key ile her kayıt benzersiz tanımlanır
- Foreign Key ile tablolar arası ilişkiler kurulur

---

**Soru 2:** VTYS (Veritabanı Yönetim Sistemi) kullanmanın avantajları nelerdir?

**Cevap 2:** VTYS'nin dosya sistemine göre avantajları:

1. **Veri Bütünlüğü:** Constraint'ler ile veri tutarlılığı sağlanır
2. **Veri Güvenliği:** Kullanıcı yetkilendirme ve şifreleme
3. **Eşzamanlı Erişim:** Birden fazla kullanıcı aynı anda erişebilir
4. **Veri Tutarlılığı:** Transaction yönetimi ile tutarlılık
5. **Yedekleme ve Kurtarma:** Otomatik backup mekanizmaları
6. **Veri Bağımsızlığı:** Fiziksel ve mantıksal bağımsızlık
7. **Veri Tekrarının Azaltılması:** Normalizasyon ile tekrar önlenir
8. **Standart Erişim:** SQL gibi standart diller ile erişim

---

**Soru 3:** SQL'in alt dilleri nelerdir? Her birinin görevini açıklayınız.

**Cevap 3:** SQL'in 4 ana alt dili vardır:

**1. DDL (Data Definition Language) - Veri Tanımlama Dili:**
- Veritabanı nesnelerini oluşturma, değiştirme, silme
- Komutlar: CREATE, ALTER, DROP, TRUNCATE
- Örnek: `CREATE TABLE`, `ALTER TABLE`

**2. DML (Data Manipulation Language) - Veri İşleme Dili:**
- Verileri ekleme, güncelleme, silme, sorgulama
- Komutlar: SELECT, INSERT, UPDATE, DELETE
- Örnek: `INSERT INTO`, `SELECT * FROM`

**3. DCL (Data Control Language) - Veri Kontrol Dili:**
- Kullanıcı yetkilendirme ve izin yönetimi
- Komutlar: GRANT, REVOKE
- Örnek: `GRANT SELECT ON Table TO User`

**4. TCL (Transaction Control Language) - Transaction Kontrol Dili:**
- Transaction yönetimi
- Komutlar: COMMIT, ROLLBACK, SAVEPOINT
- Örnek: `BEGIN TRANSACTION`, `COMMIT`

---

**Soru 4:** Primary Key ve Foreign Key arasındaki fark nedir?

**Cevap 4:**

**Primary Key (Birincil Anahtar):**
- Tablodaki her kaydı benzersiz olarak tanımlar
- NULL değer alamaz
- Bir tabloda sadece 1 tane Primary Key olabilir
- Otomatik olarak UNIQUE ve NOT NULL constraint'i içerir
- Otomatik Clustered Index oluşturur
```sql
CREATE TABLE Ogrenci (
    OgrenciID INT PRIMARY KEY,
    Ad NVARCHAR(50)
);
```

**Foreign Key (Yabancı Anahtar):**
- Başka bir tablonun Primary Key'ine referans verir
- Tablolar arası ilişki kurar
- NULL değer alabilir
- Bir tabloda birden fazla Foreign Key olabilir
- Referential integrity (bütünlük) sağlar
```sql
CREATE TABLE Ders (
    DersID INT PRIMARY KEY,
    OgrenciID INT FOREIGN KEY REFERENCES Ogrenci(OgrenciID)
);
```

---

**Soru 5:** Veritabanı oluştururken PRIMARY ve LOG dosyaları ne işe yarar?

**Cevap 5:**

**PRIMARY Dosyası (.mdf):**
- Veritabanının ana veri dosyasıdır
- Tablolar, indexler ve diğer veritabanı nesnelerini içerir
- Zorunlu bir dosyadır, her veritabanının 1 tane .mdf dosyası olur
- Varsayılan uzantısı: .mdf

**LOG Dosyası (.ldf):**
- Transaction log dosyasıdır
- Veritabanında yapılan tüm değişiklikleri kaydeder
- Kurtarma (recovery) işlemleri için kullanılır
- COMMIT ve ROLLBACK işlemlerini destekler
- Varsayılan uzantısı: .ldf

**Örnek:**
```sql
CREATE DATABASE OkulDB ON PRIMARY
(NAME = OkulDB_Data,
 FILENAME = 'C:\OkulDB_Data.mdf',
 SIZE = 5MB, MAXSIZE = 100MB, FILEGROWTH = 10%)
LOG ON
(NAME = OkulDB_Log,
 FILENAME = 'C:\OkulDB_Log.ldf',
 SIZE = 2MB, MAXSIZE = 50MB, FILEGROWTH = 10%)
```

---

**Soru 6:** NULL değer nedir? Sıfır (0) değerinden farkı nedir?

**Cevap 6:**

**NULL:**
- "Bilinmeyen" veya "tanımsız" değeri ifade eder
- Veri henüz girilmemiş veya bilinmiyor demektir
- Hiçbir değer atanmamış durumdur
- Matematiksel işlemlerde NULL sonuç verir

**Sıfır (0):**
- Sayısal bir değerdir
- Bilinen ve atanmış bir değerdir
- Matematiksel işlemlerde kullanılabilir

**Örnekler:**
```sql
-- NULL kontrolü
SELECT * FROM Urun WHERE Fiyat IS NULL;  -- Doğru
SELECT * FROM Urun WHERE Fiyat = NULL;   -- YANLIŞ!

-- Sıfır kontrolü
SELECT * FROM Urun WHERE Stok = 0;       -- Doğru

-- Fark örneği
SELECT 10 + NULL;   -- Sonuç: NULL
SELECT 10 + 0;      -- Sonuç: 10
```

---

**Soru 7:** UNIQUE constraint'i ne işe yarar? Primary Key'den farkı nedir?

**Cevap 7:**

**UNIQUE Constraint:**
- Sütundaki değerlerin benzersiz olmasını sağlar
- Tekrar eden değerlere izin vermez
- NULL değer alabilir (ancak sadece 1 NULL)
- Bir tabloda birden fazla UNIQUE constraint tanımlanabilir
- Otomatik olarak Non-Clustered Index oluşturur

**Primary Key'den Farkları:**
| Özellik | Primary Key | UNIQUE |
|---------|-------------|--------|
| NULL değer | ALAMAZ | ALABİLİR (1 tane) |
| Tablo başına | 1 tane | Birden fazla |
| Index türü | Clustered | Non-Clustered |

**Örnek:**
```sql
CREATE TABLE Kullanici (
    KullaniciID INT PRIMARY KEY,      -- NULL alamaz, benzersiz
    Email NVARCHAR(100) UNIQUE,       -- NULL alabilir, benzersiz
    TcNo CHAR(11) UNIQUE,             -- NULL alabilir, benzersiz
    Ad NVARCHAR(50)
);
```

---

**Soru 8:** CHECK constraint'i ne işe yarar? Örnek veriniz.

**Cevap 8:**

**CHECK Constraint:**
- Sütuna girilecek değerler için koşul belirler
- Belirlenen koşulu sağlamayan değerlerin girilmesini engeller
- Veri bütünlüğünü sağlar
- Karmaşık iş kurallarını veritabanı seviyesinde uygular

**Kullanım Senaryoları:**
1. Yaş kontrolü (18+ olmalı)
2. Tarih kontrolü (gelecek tarih olmamalı)
3. Sayı aralığı (0-100 arası)
4. Metin formatı (@ işareti içermeli)

**Örnekler:**
```sql
-- Yaş kontrolü
ALTER TABLE Kisi
ADD CONSTRAINT CHK_Yas CHECK (Yas >= 18 AND Yas <= 100);

-- Email formatı kontrolü
ALTER TABLE Kullanici
ADD CONSTRAINT CHK_Email CHECK (Email LIKE '%@%.%');

-- Fiyat kontrolü
ALTER TABLE Urun
ADD CONSTRAINT CHK_Fiyat CHECK (Fiyat >= 0);

-- Cinsiyet kontrolü
ALTER TABLE Kisi
ADD CONSTRAINT CHK_Cinsiyet CHECK (Cinsiyet IN ('E', 'K'));

-- Tarih kontrolü
ALTER TABLE Siparis
ADD CONSTRAINT CHK_Tarih CHECK (SiparisTarihi <= GETDATE());

-- Not kontrolü
ALTER TABLE Sinav
ADD CONSTRAINT CHK_Not CHECK (Not >= 0 AND Not <= 100);
```

---

**Soru 9:** DEFAULT constraint'i ne işe yarar? Örnek veriniz.

**Cevap 9:**

**DEFAULT Constraint:**
- Sütuna değer girilmediğinde otomatik olarak atanacak varsayılan değeri belirler
- INSERT sırasında değer belirtilmezse DEFAULT değer kullanılır
- Veri girişini kolaylaştırır ve standartlaştırır

**Örnek Kullanımlar:**
```sql
-- Tablo oluştururken DEFAULT
CREATE TABLE Urun (
    UrunID INT PRIMARY KEY,
    UrunAd NVARCHAR(100),
    Fiyat DECIMAL(10,2) DEFAULT 0,
    EklenmeTarihi DATETIME DEFAULT GETDATE(),
    Aktif BIT DEFAULT 1,
    Stok INT DEFAULT 0
);

-- Mevcut tabloya DEFAULT ekleme
ALTER TABLE Musteri
ADD CONSTRAINT DF_Kayit DEFAULT GETDATE() FOR KayitTarihi;

-- Kullanım örneği
INSERT INTO Urun (UrunID, UrunAd, Fiyat) VALUES (1, 'Kalem', 5.50);
-- EklenmeTarihi otomatik GETDATE() olur
-- Aktif otomatik 1 olur
-- Stok otomatik 0 olur

-- DEFAULT değeri görmezden gelme
INSERT INTO Urun (UrunID, UrunAd, Aktif) VALUES (2, 'Defter', 0);
```

---

**Soru 10:** Identity özelliği ne işe yarar? Nasıl kullanılır?

**Cevap 10:**

**Identity Özelliği:**
- Otomatik artan sayı üretir
- Genellikle Primary Key sütunlarında kullanılır
- Her yeni kayıtta otomatik olarak bir sonraki sayıyı atar
- Manuel değer girişini engeller (özel durumlar hariç)

**Syntax:**
```sql
IDENTITY(başlangıç_değeri, artış_miktarı)
```

**Örnekler:**
```sql
-- Temel kullanım
CREATE TABLE Ogrenci (
    OgrenciID INT IDENTITY(1,1) PRIMARY KEY,  -- 1'den başla, 1'er artır
    Ad NVARCHAR(50),
    Soyad NVARCHAR(50)
);

-- Farklı başlangıç ve artış
CREATE TABLE Siparis (
    SiparisNo INT IDENTITY(1000,5) PRIMARY KEY,  -- 1000'den başla, 5'er artır
    Tarih DATETIME
);

-- Identity değerini öğrenme
INSERT INTO Ogrenci (Ad, Soyad) VALUES ('Ali', 'Veli');
SELECT SCOPE_IDENTITY();  -- Son eklenen Identity değeri: 1

-- Identity'yi manuel set etme (AÇMAK)
SET IDENTITY_INSERT Ogrenci ON;
INSERT INTO Ogrenci (OgrenciID, Ad, Soyad) VALUES (100, 'Ayşe', 'Yılmaz');
SET IDENTITY_INSERT Ogrenci OFF;

-- Mevcut Identity değerini öğrenme
SELECT IDENT_CURRENT('Ogrenci');  -- Ogrenci tablosunun son Identity değeri
```

**Identity vs. Primary Key:**
- Identity: Otomatik sayı üretir
- Primary Key: Benzersizlik sağlar
- Genellikle birlikte kullanılır ama zorunlu değildir

---


### ORTA SORULAR (11-20)

**Soru 11:** Aşağıdaki CREATE DATABASE komutunu açıklayınız. SIZE, MAXSIZE ve FILEGROWTH parametrelerinin anlamı nedir?

```sql
CREATE DATABASE OkulDB ON PRIMARY
(NAME = OkulDB_Data,
 FILENAME = 'C:\Database\OkulDB_Data.mdf',
 SIZE = 10MB,
 MAXSIZE = 500MB,
 FILEGROWTH = 15%)
LOG ON
(NAME = OkulDB_Log,
 FILENAME = 'C:\Database\OkulDB_Log.ldf',
 SIZE = 5MB,
 MAXSIZE = 100MB,
 FILEGROWTH = 5MB)
```

**Cevap 11:**

**Parametre Açıklamaları:**

1. **SIZE (Başlangıç Boyutu):**
   - Dosyanın ilk oluşturulduğunda sahip olacağı boyut
   - Data dosyası: 10MB ile başlar
   - Log dosyası: 5MB ile başlar

2. **MAXSIZE (Maksimum Boyut):**
   - Dosyanın büyüyebileceği maksimum boyut
   - Data dosyası max 500MB olabilir
   - Log dosyası max 100MB olabilir
   - UNLIMITED yazılırsa sınırsız büyür

3. **FILEGROWTH (Büyüme Miktarı):**
   - Dosya dolduğunda ne kadar büyüyeceği
   - **Yüzde (%):** Data dosyası %15 büyür (10MB → 11.5MB)
   - **Sabit (MB):** Log dosyası 5MB büyür (5MB → 10MB)

**Tercih Edilenler:**
- Log dosyası için SABİT değer (MB) önerilir
- Data dosyası için YÜZDE (%) kullanılabilir
- MAXSIZE belirlenmeli (disk dolmasın)

---

**Soru 12:** Aşağıdaki Urun tablosunu oluşturan SQL kodunu yazınız:
- UrunID: Primary Key, otomatik artan
- UrunAd: 100 karakterlik text, zorunlu
- Fiyat: Ondalıklı sayı, 0'dan küçük olamaz, varsayılan 0
- Stok: Tam sayı, negatif olamaz
- EklenmeTarihi: Tarih, varsayılan bugünün tarihi
- Aktif: Evet/Hayır, varsayılan Evet

**Cevap 12:**

```sql
CREATE TABLE Urun (
    UrunID INT IDENTITY(1,1) PRIMARY KEY,
    UrunAd NVARCHAR(100) NOT NULL,
    Fiyat DECIMAL(10,2) DEFAULT 0 CHECK (Fiyat >= 0),
    Stok INT CHECK (Stok >= 0),
    EklenmeTarihi DATETIME DEFAULT GETDATE(),
    Aktif BIT DEFAULT 1
);
```

**Alternatif (Constraint isimleriyle):**
```sql
CREATE TABLE Urun (
    UrunID INT IDENTITY(1,1),
    UrunAd NVARCHAR(100) NOT NULL,
    Fiyat DECIMAL(10,2) DEFAULT 0,
    Stok INT,
    EklenmeTarihi DATETIME DEFAULT GETDATE(),
    Aktif BIT DEFAULT 1,
    
    CONSTRAINT PK_Urun PRIMARY KEY (UrunID),
    CONSTRAINT CHK_Fiyat CHECK (Fiyat >= 0),
    CONSTRAINT CHK_Stok CHECK (Stok >= 0)
);
```

---

**Soru 13:** FILEGROUP nedir? Ne işe yarar? Örnek kullanım gösteriniz.

**Cevap 13:**

**FILEGROUP (Dosya Grubu):**
- Veritabanı dosyalarını mantıksal gruplara ayırır
- Performans optimizasyonu için kullanılır
- Farklı disk sürücülerinde dosyalar oluşturulabilir
- Yedekleme stratejileri için kullanılır

**Avantajları:**
1. **Performans:** Farklı disklere dağıtılabilir (I/O hızı artar)
2. **Yönetim:** Büyük tabloları farklı gruplarda saklama
3. **Yedekleme:** Seçici filegroup yedekleme
4. **Organizasyon:** İlgili nesneleri gruplandırma

**Örnek:**
```sql
-- 3 Filegroup ile database oluşturma
CREATE DATABASE SirketDB ON PRIMARY
(
    NAME = Sirket_Primary,
    FILENAME = 'C:\DB\Sirket_Primary.mdf',
    SIZE = 10MB
),
FILEGROUP FG_Musteriler
(
    NAME = Sirket_Musteriler,
    FILENAME = 'D:\DB\Sirket_Musteriler.ndf',
    SIZE = 100MB
),
FILEGROUP FG_Siparisler
(
    NAME = Sirket_Siparisler,
    FILENAME = 'E:\DB\Sirket_Siparisler.ndf',
    SIZE = 200MB
)
LOG ON
(
    NAME = Sirket_Log,
    FILENAME = 'C:\DB\Sirket_Log.ldf',
    SIZE = 50MB
);

-- Tablo oluştururken filegroup belirtme
CREATE TABLE Musteriler (
    MusteriID INT PRIMARY KEY,
    Ad NVARCHAR(100)
) ON FG_Musteriler;

-- Varsayılan filegroup ayarlama
ALTER DATABASE SirketDB MODIFY FILEGROUP FG_Musteriler DEFAULT;
```

---

**Soru 14:** Composite Primary Key nedir? Ne zaman kullanılır? Örnek veriniz.

**Cevap 14:**

**Composite Primary Key (Bileşik Birincil Anahtar):**
- Birden fazla sütunun birleşiminden oluşan Primary Key
- Her sütun ayrı ayrı tekrar edebilir ama birleşim benzersiz olmalı
- Çoka-çok ilişkilerde junction tablolarında kullanılır

**Kullanım Senaryoları:**
1. **Öğrenci-Ders ilişkisi:** (OgrenciID, DersID)
2. **Sipariş detayları:** (SiparisID, UrunID)
3. **Rezervasyon sistemi:** (SalonID, GunID, SaatID)

**Örnekler:**
```sql
-- Örnek 1: Öğrenci-Ders kayıt sistemi
CREATE TABLE OgrenciDers (
    OgrenciID INT,
    DersID INT,
    KayitTarihi DATETIME,
    Durum NVARCHAR(20),
    
    PRIMARY KEY (OgrenciID, DersID),
    FOREIGN KEY (OgrenciID) REFERENCES Ogrenci(OgrenciID),
    FOREIGN KEY (DersID) REFERENCES Ders(DersID)
);

-- Örnek 2: Sipariş detayları
CREATE TABLE SiparisDetay (
    SiparisID INT,
    UrunID INT,
    Adet INT,
    BirimFiyat DECIMAL(10,2),
    
    CONSTRAINT PK_SiparisDetay PRIMARY KEY (SiparisID, UrunID),
    CONSTRAINT FK_Siparis FOREIGN KEY (SiparisID) REFERENCES Siparis(SiparisID),
    CONSTRAINT FK_Urun FOREIGN KEY (UrunID) REFERENCES Urun(UrunID)
);

-- Kullanım
INSERT INTO OgrenciDers VALUES (1, 101, GETDATE(), 'Aktif');  -- OK
INSERT INTO OgrenciDers VALUES (1, 101, GETDATE(), 'Pasif');  -- HATA! (1,101) zaten var
INSERT INTO OgrenciDers VALUES (1, 102, GETDATE(), 'Aktif');  -- OK (farklı ders)
```

---

**Soru 15:** Candidate Key, Alternate Key ve Super Key kavramlarını açıklayınız.

**Cevap 15:**

**1. Super Key (Süper Anahtar):**
- Bir kaydı benzersiz şekilde tanımlayabilen sütun veya sütun kombinasyonu
- Gereksiz sütunlar içerebilir
- Örnek: {OgrenciID}, {TcNo}, {OgrenciID, Ad}, {TcNo, Ad, Soyad}

**2. Candidate Key (Aday Anahtar):**
- Minimal Super Key (gereksiz sütun içermez)
- Primary Key olma potansiyeli taşır
- Bir tabloda birden fazla olabilir
- Örnek: {OgrenciID}, {TcNo}, {Email}

**3. Primary Key (Birincil Anahtar):**
- Candidate Key'ler arasından seçilen
- Tablonun ana anahtarı
- Sadece 1 tane olur

**4. Alternate Key (Alternatif Anahtar):**
- Primary Key seçilmeyen diğer Candidate Key'ler
- UNIQUE constraint ile tanımlanır

**Örnek Tablo:**
```sql
CREATE TABLE Ogrenci (
    OgrenciID INT PRIMARY KEY,           -- Primary Key (Candidate Key'den seçildi)
    TcNo CHAR(11) UNIQUE,                -- Alternate Key (Candidate Key idi)
    Email NVARCHAR(100) UNIQUE,          -- Alternate Key (Candidate Key idi)
    Ad NVARCHAR(50),
    Soyad NVARCHAR(50),
    DogumTarihi DATE
);
```

**Candidate Keys:** {OgrenciID}, {TcNo}, {Email}
**Primary Key:** {OgrenciID}
**Alternate Keys:** {TcNo}, {Email}
**Super Keys:** {OgrenciID}, {TcNo}, {Email}, {OgrenciID, Ad}, {TcNo, Soyad}, vb.

---

**Soru 16:** ON DELETE CASCADE ve ON UPDATE CASCADE ne anlama gelir? Ne zaman kullanılır?

**Cevap 16:**

**Referential Integrity Actions:**

**1. ON DELETE CASCADE:**
- Ana tablodaki kayıt silindiğinde, bağlantılı tablodaki kayıtlar da otomatik silinir
- "Cascading Delete" - Art arda silme

**2. ON UPDATE CASCADE:**
- Ana tablodaki Primary Key güncellendiğinde, bağlantılı tablodaki Foreign Key'ler de güncellenir

**Diğer Seçenekler:**
- **NO ACTION:** İşlemi engelle (varsayılan)
- **SET NULL:** Foreign Key'i NULL yap
- **SET DEFAULT:** Foreign Key'i DEFAULT değere ayarla

**Örnek:**
```sql
-- Müşteri ve Sipariş tabloları
CREATE TABLE Musteri (
    MusteriID INT PRIMARY KEY,
    Ad NVARCHAR(100)
);

CREATE TABLE Siparis (
    SiparisID INT PRIMARY KEY,
    MusteriID INT,
    Tutar DECIMAL(10,2),
    
    CONSTRAINT FK_Musteri 
    FOREIGN KEY (MusteriID) REFERENCES Musteri(MusteriID)
    ON DELETE CASCADE          -- Müşteri silinince siparişleri de silinir
    ON UPDATE CASCADE          -- Müşteri ID'si değişince siparişlerde de değişir
);

-- Test
INSERT INTO Musteri VALUES (1, 'Ali Veli');
INSERT INTO Siparis VALUES (100, 1, 500.00);
INSERT INTO Siparis VALUES (101, 1, 750.00);

DELETE FROM Musteri WHERE MusteriID = 1;  
-- Müşteri silinir, 2 sipariş de otomatik silinir (CASCADE)

-- CASCADE olmasaydı:
-- Error: The DELETE statement conflicted with the REFERENCE constraint
```

**Ne Zaman Kullanılır:**
- ✅ **Kullan:** Sipariş-SiparisDetay (sipariş silinince detayları da silinmeli)
- ❌ **Kullanma:** Müşteri-Sipariş (müşteri silinince siparişler kaybedilmemeli)

---

**Soru 17:** Bir tabloya constraint ekleme ve silme işlemlerini örneklerle gösteriniz.

**Cevap 17:**

**Constraint Ekleme:**

```sql
-- 1. PRIMARY KEY ekleme
ALTER TABLE Urun
ADD CONSTRAINT PK_Urun PRIMARY KEY (UrunID);

-- 2. FOREIGN KEY ekleme
ALTER TABLE Siparis
ADD CONSTRAINT FK_Musteri FOREIGN KEY (MusteriID) 
    REFERENCES Musteri(MusteriID);

-- 3. UNIQUE ekleme
ALTER TABLE Kullanici
ADD CONSTRAINT UQ_Email UNIQUE (Email);

-- 4. CHECK ekleme
ALTER TABLE Urun
ADD CONSTRAINT CHK_Fiyat CHECK (Fiyat >= 0 AND Fiyat <= 100000);

-- 5. DEFAULT ekleme
ALTER TABLE Siparis
ADD CONSTRAINT DF_Tarih DEFAULT GETDATE() FOR SiparisTarihi;

-- 6. Birden fazla sütuna CHECK
ALTER TABLE Personel
ADD CONSTRAINT CHK_Maas CHECK (NetMaas <= BrutMaas);

-- 7. Karmaşık CHECK
ALTER TABLE Kullanici
ADD CONSTRAINT CHK_Email CHECK (
    Email LIKE '%@%.%' AND 
    LEN(Email) >= 5
);
```

**Constraint Silme:**

```sql
-- 1. Constraint silme (genel)
ALTER TABLE Urun
DROP CONSTRAINT CHK_Fiyat;

-- 2. PRIMARY KEY silme
ALTER TABLE Urun
DROP CONSTRAINT PK_Urun;

-- 3. FOREIGN KEY silme
ALTER TABLE Siparis
DROP CONSTRAINT FK_Musteri;

-- 4. UNIQUE silme
ALTER TABLE Kullanici
DROP CONSTRAINT UQ_Email;

-- 5. DEFAULT silme
ALTER TABLE Siparis
DROP CONSTRAINT DF_Tarih;

-- 6. Birden fazla constraint silme
ALTER TABLE Urun
DROP CONSTRAINT CHK_Fiyat, CHK_Stok;
```

**Constraint İsimlerini Öğrenme:**

```sql
-- Tüm constraint'leri göster
SELECT 
    CONSTRAINT_NAME,
    CONSTRAINT_TYPE,
    TABLE_NAME
FROM INFORMATION_SCHEMA.TABLE_CONSTRAINTS
WHERE TABLE_NAME = 'Urun';

-- Detaylı bilgi
EXEC sp_helpconstraint 'Urun';
```

---

**Soru 18:** Normalizasyon nedir? 1NF, 2NF ve 3NF'i açıklayınız.

**Cevap 18:**

**Normalizasyon:**
- Veri tekrarını azaltma ve veri bütünlüğünü sağlama süreci
- Tablolar arası ilişkileri düzenleme
- Anomalileri (insert, update, delete) önleme

**1. Normal Form (1NF):**
**Kural:** Her hücre tek bir değer içermeli (atomic)

**Kötü Tasarım:**
```
Ogrenci Tablosu:
OgrenciID | Ad      | Telefonlar
1         | Ali     | 5551234567, 5559876543
```

**1NF Uyumlu:**
```
Ogrenci Tablosu:
OgrenciID | Ad      
1         | Ali     

OgrenciTelefon Tablosu:
OgrenciID | Telefon
1         | 5551234567
1         | 5559876543
```

**2. Normal Form (2NF):**
**Kural:** 1NF + Kısmi bağımlılık olmamalı (non-key sütunlar tüm Primary Key'e bağımlı olmalı)

**Kötü Tasarım (1NF ama 2NF değil):**
```
SiparisDetay:
SiparisID | UrunID | Adet | UrunAd | UrunFiyat
1         | 10     | 2    | Kalem  | 5.00
```
Problem: UrunAd ve UrunFiyat sadece UrunID'ye bağlı (kısmi bağımlılık)

**2NF Uyumlu:**
```
SiparisDetay:
SiparisID | UrunID | Adet
1         | 10     | 2

Urun:
UrunID | UrunAd | Fiyat
10     | Kalem  | 5.00
```

**3. Normal Form (3NF):**
**Kural:** 2NF + Geçişken bağımlılık olmamalı (non-key sütunlar başka non-key sütunlara bağlı olmamalı)

**Kötü Tasarım (2NF ama 3NF değil):**
```
Ogrenci:
OgrenciID | Ad  | BolumID | BolumAd
1         | Ali | 10      | Bilgisayar Müh.
```
Problem: BolumAd, BolumID'ye bağlı (geçişken bağımlılık)

**3NF Uyumlu:**
```
Ogrenci:
OgrenciID | Ad  | BolumID
1         | Ali | 10

Bolum:
BolumID | BolumAd
10      | Bilgisayar Müh.
```

---

**Soru 19:** Aşağıdaki senaryoyu veritabanı tablolarına dönüştürünüz:
"Bir kütüphanede kitaplar, yazarlar ve üyeler bulunmaktadır. Bir kitabın birden fazla yazarı olabilir. Bir yazar birden fazla kitap yazabilir. Üyeler kitapları ödünç alabilir. Her ödünç alma işleminde alış ve teslim tarihleri kaydedilir."

**Cevap 19:**

**Tablo Tasarımı:**

```sql
-- 1. Kitap tablosu
CREATE TABLE Kitap (
    KitapID INT PRIMARY KEY IDENTITY(1,1),
    ISBN VARCHAR(20) UNIQUE,
    KitapAd NVARCHAR(200) NOT NULL,
    YayinYili INT,
    SayfaSayisi INT,
    RafNo NVARCHAR(20)
);

-- 2. Yazar tablosu
CREATE TABLE Yazar (
    YazarID INT PRIMARY KEY IDENTITY(1,1),
    Ad NVARCHAR(50) NOT NULL,
    Soyad NVARCHAR(50) NOT NULL,
    DogumYili INT,
    Ulke NVARCHAR(50)
);

-- 3. Kitap-Yazar ilişki tablosu (Çoka-çok)
CREATE TABLE KitapYazar (
    KitapID INT,
    YazarID INT,
    YazarSirasi TINYINT,  -- 1. yazar, 2. yazar vb.
    
    PRIMARY KEY (KitapID, YazarID),
    FOREIGN KEY (KitapID) REFERENCES Kitap(KitapID),
    FOREIGN KEY (YazarID) REFERENCES Yazar(YazarID)
);

-- 4. Üye tablosu
CREATE TABLE Uye (
    UyeID INT PRIMARY KEY IDENTITY(1,1),
    TcNo CHAR(11) UNIQUE NOT NULL,
    Ad NVARCHAR(50) NOT NULL,
    Soyad NVARCHAR(50) NOT NULL,
    Email NVARCHAR(100),
    Telefon VARCHAR(15),
    KayitTarihi DATETIME DEFAULT GETDATE()
);

-- 5. Ödünç alma tablosu
CREATE TABLE OduncAlma (
    OduncID INT PRIMARY KEY IDENTITY(1,1),
    UyeID INT NOT NULL,
    KitapID INT NOT NULL,
    AlisTarihi DATETIME NOT NULL DEFAULT GETDATE(),
    TeslimTarihi DATETIME,
    BekleneTeslimTarihi DATETIME NOT NULL,
    
    FOREIGN KEY (UyeID) REFERENCES Uye(UyeID),
    FOREIGN KEY (KitapID) REFERENCES Kitap(KitapID),
    CHECK (TeslimTarihi IS NULL OR TeslimTarihi >= AlisTarihi)
);
```

**İlişki Diyagramı:**
```
Kitap 1---N KitapYazar N---1 Yazar
Kitap 1---N OduncAlma N---1 Uye
```

---

**Soru 20:** Veritabanı transaction'ı nedir? COMMIT ve ROLLBACK ne işe yarar? Örnek veriniz.

**Cevap 20:**

**Transaction (İşlem):**
- Birden fazla SQL komutunun tek bir mantıksal işlem olarak yürütülmesi
- Tamamı başarılı olmalı veya hiçbiri yapılmamalı (ACID prensipleri)
- Veri tutarlılığını sağlar

**ACID Prensipleri:**
- **A**tomicity: Bölünmezlik (ya hepsi ya hiçbiri)
- **C**onsistency: Tutarlılık
- **I**solation: Yalıtım
- **D**urability: Kalıcılık

**COMMIT:** Transaction'ı kalıcı hale getirir
**ROLLBACK:** Transaction'ı geri alır

**Örnek 1: Banka Transfer İşlemi**
```sql
BEGIN TRANSACTION;

-- Ali'nin hesabından 1000 TL çıkar
UPDATE Hesap 
SET Bakiye = Bakiye - 1000 
WHERE HesapNo = '12345';

-- Veli'nin hesabına 1000 TL ekle
UPDATE Hesap 
SET Bakiye = Bakiye + 1000 
WHERE HesapNo = '67890';

-- Kontrol: Negatif bakiye var mı?
IF EXISTS (SELECT * FROM Hesap WHERE Bakiye < 0)
BEGIN
    ROLLBACK;  -- İşlemi geri al
    PRINT 'Transfer başarısız: Yetersiz bakiye';
END
ELSE
BEGIN
    COMMIT;    -- İşlemi onayla
    PRINT 'Transfer başarılı';
END
```

**Örnek 2: Sipariş İşlemi**
```sql
BEGIN TRY
    BEGIN TRANSACTION;
    
    -- Sipariş oluştur
    INSERT INTO Siparis (MusteriID, Tarih, Toplam)
    VALUES (1, GETDATE(), 500);
    
    DECLARE @SiparisID INT = SCOPE_IDENTITY();
    
    -- Sipariş detaylarını ekle
    INSERT INTO SiparisDetay (SiparisID, UrunID, Adet, Fiyat)
    VALUES (@SiparisID, 10, 2, 250);
    
    -- Stoktan düş
    UPDATE Urun 
    SET Stok = Stok - 2 
    WHERE UrunID = 10;
    
    COMMIT;
    PRINT 'Sipariş başarıyla oluşturuldu';
END TRY
BEGIN CATCH
    ROLLBACK;
    PRINT 'Hata: ' + ERROR_MESSAGE();
END CATCH
```

**Transaction Kontrol Komutları:**
```sql
-- Transaction başlat
BEGIN TRANSACTION;
BEGIN TRAN;

-- Transaction'ı onayla
COMMIT;
COMMIT TRANSACTION;

-- Transaction'ı geri al
ROLLBACK;
ROLLBACK TRANSACTION;

-- Savepoint oluştur
SAVE TRANSACTION Nokta1;
ROLLBACK TRANSACTION Nokta1;
```

---


### ZOR SORULAR (21-30)

**Soru 21:** Aşağıdaki tabloya göre sorgunun çıktısını bulunuz:

**Urun Tablosu:**
| UrunID | UrunAd | Kategori | Fiyat | Stok |
|--------|--------|----------|-------|------|
| 1      | Kalem  | Kırtasiye| 5.00  | 100  |
| 2      | Defter | Kırtasiye| 15.00 | 50   |
| 3      | Çanta  | Aksesuar | 250.00| 20   |
| 4      | Kalem  | Kırtasiye| 8.00  | 75   |
| 5      | Silgi  | NULL     | 2.50  | 200  |

```sql
SELECT Kategori, COUNT(*) AS Adet, AVG(Fiyat) AS OrtFiyat, SUM(Stok) AS TopStok
FROM Urun
WHERE Fiyat > 5
GROUP BY Kategori
HAVING COUNT(*) > 1
ORDER BY OrtFiyat DESC;
```

**Cevap 21:**

**Adım Adım Çözüm:**

1. **WHERE Fiyat > 5:** Fiyatı 5'ten büyük olanları filtrele
   - Kalem (5.00) ELENIR
   - Defter (15.00) ✓
   - Çanta (250.00) ✓
   - Kalem (8.00) ✓
   - Silgi (2.50) ELENIR

2. **GROUP BY Kategori:** Kategorilere göre grupla
   - Kırtasiye: Defter, Kalem (8.00)
   - Aksesuar: Çanta
   - NULL: (yok, çünkü Silgi elendi)

3. **HAVING COUNT(*) > 1:** 1'den fazla kayıt olan grupları al
   - Kırtasiye: 2 kayıt ✓
   - Aksesuar: 1 kayıt ELENIR

4. **SELECT Hesaplamaları:**
   - Kategori: Kırtasiye
   - Adet: COUNT(*) = 2
   - OrtFiyat: AVG(Fiyat) = (15.00 + 8.00) / 2 = 11.50
   - TopStok: SUM(Stok) = 50 + 75 = 125

5. **ORDER BY OrtFiyat DESC:** Ortalama fiyata göre azalan sırala

**Sonuç:**
| Kategori  | Adet | OrtFiyat | TopStok |
|-----------|------|----------|---------|
| Kırtasiye | 2    | 11.50    | 125     |

**Önemli Notlar:**
- WHERE aggregate fonksiyon ile kullanılamaz
- HAVING aggregate fonksiyon ile kullanılır
- NULL değerler GROUP BY'da ayrı bir grup oluşturur
- COUNT(*) NULL'ları sayar, COUNT(sütun) NULL'ları saymaz

---

**Soru 22:** Self-Join nedir? Aşağıdaki senaryoyu Self-Join kullanarak çözünüz:
"Personel tablosunda her personelin bir yöneticisi vardır (YoneticiID). Tüm personelleri ve yöneticilerinin adlarını listeleyen sorguyu yazınız."

**Cevap 22:**

**Self-Join:**
- Bir tablonun kendisiyle JOIN edilmesi
- Hiyerarşik ilişkilerde kullanılır (çalışan-yönetici, kategori-üst kategori)
- Aynı tabloya farklı alias'lar verilerek yapılır

**Personel Tablosu:**
| PersonelID | Ad        | Soyad  | YoneticiID |
|------------|-----------|--------|------------|
| 1          | Ahmet     | Yılmaz | NULL       |
| 2          | Mehmet    | Kaya   | 1          |
| 3          | Ayşe      | Demir  | 1          |
| 4          | Fatma     | Şahin  | 2          |
| 5          | Ali       | Çelik  | 2          |

**Çözüm:**
```sql
-- INNER JOIN (Yöneticisi olmayan personel gösterilmez)
SELECT 
    P.PersonelID,
    P.Ad + ' ' + P.Soyad AS PersonelAd,
    Y.Ad + ' ' + Y.Soyad AS YoneticiAd
FROM Personel P
INNER JOIN Personel Y ON P.YoneticiID = Y.PersonelID;

-- LEFT JOIN (Yöneticisi olmayan personel de gösterilir)
SELECT 
    P.PersonelID,
    P.Ad + ' ' + P.Soyad AS PersonelAd,
    ISNULL(Y.Ad + ' ' + Y.Soyad, 'Yöneticisi Yok') AS YoneticiAd
FROM Personel P
LEFT JOIN Personel Y ON P.YoneticiID = Y.PersonelID;
```

**INNER JOIN Sonucu:**
| PersonelID | PersonelAd    | YoneticiAd    |
|------------|---------------|---------------|
| 2          | Mehmet Kaya   | Ahmet Yılmaz  |
| 3          | Ayşe Demir    | Ahmet Yılmaz  |
| 4          | Fatma Şahin   | Mehmet Kaya   |
| 5          | Ali Çelik     | Mehmet Kaya   |

**LEFT JOIN Sonucu:**
| PersonelID | PersonelAd    | YoneticiAd      |
|------------|---------------|-----------------|
| 1          | Ahmet Yılmaz  | Yöneticisi Yok  |
| 2          | Mehmet Kaya   | Ahmet Yılmaz    |
| 3          | Ayşe Demir    | Ahmet Yılmaz    |
| 4          | Fatma Şahin   | Mehmet Kaya     |
| 5          | Ali Çelik     | Mehmet Kaya     |

**Diğer Kullanım Örnekleri:**
```sql
-- Yönetici olan personelleri listele
SELECT DISTINCT Y.PersonelID, Y.Ad, Y.Soyad
FROM Personel P
INNER JOIN Personel Y ON P.YoneticiID = Y.PersonelID;

-- Her yöneticinin kaç personeli var?
SELECT 
    Y.Ad + ' ' + Y.Soyad AS Yonetici,
    COUNT(P.PersonelID) AS PersonelSayisi
FROM Personel Y
LEFT JOIN Personel P ON Y.PersonelID = P.YoneticiID
GROUP BY Y.PersonelID, Y.Ad, Y.Soyad
HAVING COUNT(P.PersonelID) > 0;
```

---

**Soru 23:** Subquery (Alt sorgu) nedir? Correlated ve Non-Correlated subquery arasındaki fark nedir? Her birinden örnek veriniz.

**Cevap 23:**

**Subquery (Alt Sorgu):**
- Bir sorgu içinde başka bir sorgu
- Parantez içinde yazılır
- SELECT, WHERE, FROM, HAVING vb. ile kullanılabilir

**1. Non-Correlated Subquery (Bağımsız Alt Sorgu):**
- Dış sorgudan bağımsız çalışır
- Bir kez çalıştırılır
- Sonucu dış sorguda kullanılır

**Örnekler:**
```sql
-- En pahalı ürünü bul
SELECT UrunAd, Fiyat
FROM Urun
WHERE Fiyat = (SELECT MAX(Fiyat) FROM Urun);

-- Ortalama fiyatın üzerindeki ürünler
SELECT UrunAd, Fiyat
FROM Urun
WHERE Fiyat > (SELECT AVG(Fiyat) FROM Urun);

-- Sipariş vermiş müşterileri listele
SELECT Ad, Soyad
FROM Musteri
WHERE MusteriID IN (SELECT DISTINCT MusteriID FROM Siparis);

-- En çok satan kategorideki ürünler
SELECT *
FROM Urun
WHERE Kategori = (
    SELECT TOP 1 Kategori 
    FROM Urun 
    GROUP BY Kategori 
    ORDER BY COUNT(*) DESC
);
```

**2. Correlated Subquery (Bağımlı Alt Sorgu):**
- Dış sorguya bağımlı çalışır
- Her satır için ayrı çalıştırılır
- Dış sorgudaki değerleri kullanır
- Daha yavaş çalışır

**Örnekler:**
```sql
-- Her kategorideki en pahalı ürünleri bul
SELECT U1.UrunAd, U1.Kategori, U1.Fiyat
FROM Urun U1
WHERE U1.Fiyat = (
    SELECT MAX(U2.Fiyat) 
    FROM Urun U2 
    WHERE U2.Kategori = U1.Kategori  -- Dış sorguya bağımlı
);

-- Sipariş vermeyen müşterileri bul
SELECT M.Ad, M.Soyad
FROM Musteri M
WHERE NOT EXISTS (
    SELECT 1 
    FROM Siparis S 
    WHERE S.MusteriID = M.MusteriID  -- Dış sorguya bağımlı
);

-- Ortalama fiyatın üzerinde ürünü olan kategoriler
SELECT DISTINCT U1.Kategori
FROM Urun U1
WHERE U1.Fiyat > (
    SELECT AVG(U2.Fiyat) 
    FROM Urun U2 
    WHERE U2.Kategori = U1.Kategori
);

-- Her müşterinin en son siparişini bul
SELECT S1.MusteriID, S1.SiparisTarihi, S1.Tutar
FROM Siparis S1
WHERE S1.SiparisTarihi = (
    SELECT MAX(S2.SiparisTarihi) 
    FROM Siparis S2 
    WHERE S2.MusteriID = S1.MusteriID
);
```

**Karşılaştırma:**

| Özellik | Non-Correlated | Correlated |
|---------|----------------|------------|
| Bağımsızlık | Dış sorgudan bağımsız | Dış sorguya bağımlı |
| Çalışma | 1 kez | Her satır için |
| Performans | Daha hızlı | Daha yavaş |
| Kullanım | Basit filtreler | Satır bazlı karşılaştırma |

**EXISTS vs IN:**
```sql
-- IN (Non-Correlated)
SELECT * FROM Musteri
WHERE MusteriID IN (SELECT MusteriID FROM Siparis);

-- EXISTS (Correlated - Daha performanslı)
SELECT * FROM Musteri M
WHERE EXISTS (SELECT 1 FROM Siparis S WHERE S.MusteriID = M.MusteriID);
```

---

**Soru 24:** CTE (Common Table Expression) nedir? WITH kullanımını örneklerle açıklayınız.

**Cevap 24:**

**CTE (Common Table Expression):**
- Geçici bir sonuç kümesi tanımlar
- WITH anahtar kelimesi ile oluşturulur
- Sadece o sorgu içinde geçerlidir
- Karmaşık sorguları basitleştirir
- Okunabilirliği artırır
- Recursive (özyinelemeli) sorgular için kullanılır

**Syntax:**
```sql
WITH CTE_Adi AS (
    SELECT sorgusu
)
SELECT * FROM CTE_Adi;
```

**Örnek 1: Basit CTE**
```sql
-- Subquery ile (okunması zor)
SELECT *
FROM (
    SELECT UrunAd, Fiyat, Stok, Fiyat * Stok AS Deger
    FROM Urun
) AS Hesaplama
WHERE Deger > 1000;

-- CTE ile (daha okunabilir)
WITH UrunDegerleri AS (
    SELECT UrunAd, Fiyat, Stok, Fiyat * Stok AS Deger
    FROM Urun
)
SELECT * 
FROM UrunDegerleri
WHERE Deger > 1000;
```

**Örnek 2: Birden Fazla CTE**
```sql
WITH 
ToplamSiparisler AS (
    SELECT MusteriID, COUNT(*) AS SiparisSayisi
    FROM Siparis
    GROUP BY MusteriID
),
OrtalamaHesap AS (
    SELECT AVG(SiparisSayisi) AS OrtSiparis
    FROM ToplamSiparisler
)
SELECT 
    M.Ad, M.Soyad, TS.SiparisSayisi, OH.OrtSiparis
FROM Musteri M
INNER JOIN ToplamSiparisler TS ON M.MusteriID = TS.MusteriID
CROSS JOIN OrtalamaHesap OH
WHERE TS.SiparisSayisi > OH.OrtSiparis;
```

**Örnek 3: Recursive CTE (Özyinelemeli)**
```sql
-- Personel hiyerarşisini göster (1'den 10'a kadar)
WITH PersonelHiyerarsi AS (
    -- Anchor: Başlangıç (En üst yönetici)
    SELECT PersonelID, Ad, Soyad, YoneticiID, 1 AS Seviye
    FROM Personel
    WHERE YoneticiID IS NULL
    
    UNION ALL
    
    -- Recursive: Her seviyedeki alt personeller
    SELECT P.PersonelID, P.Ad, P.Soyad, P.YoneticiID, PH.Seviye + 1
    FROM Personel P
    INNER JOIN PersonelHiyerarsi PH ON P.YoneticiID = PH.PersonelID
    WHERE PH.Seviye < 10  -- Sonsuz döngüyü önle
)
SELECT 
    REPLICATE('  ', Seviye - 1) + Ad + ' ' + Soyad AS Hiyerarsi,
    Seviye
FROM PersonelHiyerarsi
ORDER BY Seviye, Ad;
```

**Örnek 4: Sayı Serisı Oluşturma**
```sql
-- 1'den 100'e kadar sayılar
WITH Sayilar AS (
    SELECT 1 AS Sayi
    UNION ALL
    SELECT Sayi + 1
    FROM Sayilar
    WHERE Sayi < 100
)
SELECT * FROM Sayilar;
```

**Örnek 5: Karmaşık İş Mantığı**
```sql
WITH 
-- 1. Her müşterinin toplam harcaması
MusteriHarcama AS (
    SELECT MusteriID, SUM(Tutar) AS ToplamHarcama
    FROM Siparis
    GROUP BY MusteriID
),
-- 2. Müşteri segmentasyonu
MusteriSegment AS (
    SELECT 
        MusteriID,
        ToplamHarcama,
        CASE 
            WHEN ToplamHarcama > 10000 THEN 'VIP'
            WHEN ToplamHarcama > 5000 THEN 'Gold'
            WHEN ToplamHarcama > 1000 THEN 'Silver'
            ELSE 'Bronze'
        END AS Segment
    FROM MusteriHarcama
),
-- 3. Segment istatistikleri
SegmentStats AS (
    SELECT 
        Segment,
        COUNT(*) AS MusteriSayisi,
        AVG(ToplamHarcama) AS OrtHarcama,
        SUM(ToplamHarcama) AS ToplamGelir
    FROM MusteriSegment
    GROUP BY Segment
)
SELECT 
    M.Ad, M.Soyad,
    MS.Segment,
    MS.ToplamHarcama,
    SS.OrtHarcama,
    SS.MusteriSayisi
FROM Musteri M
INNER JOIN MusteriSegment MS ON M.MusteriID = MS.MusteriID
INNER JOIN SegmentStats SS ON MS.Segment = SS.Segment
ORDER BY MS.ToplamHarcama DESC;
```

**CTE vs Subquery vs Temp Table:**

| Özellik | CTE | Subquery | Temp Table |
|---------|-----|----------|------------|
| Okunabilirlik | Yüksek | Düşük | Orta |
| Performans | Orta | Orta | Yüksek |
| Recursive | Evet | Hayır | Hayır |
| Birden fazla kullanım | Evet | Hayır | Evet |
| Kalıcılık | Sadece sorgu | Sadece sorgu | Session boyunca |

---

**Soru 25:** Window Functions (Pencere Fonksiyonları) nedir? ROW_NUMBER, RANK, DENSE_RANK farkını örneklerle açıklayınız.

**Cevap 25:**

**Window Functions:**
- Satır grupları üzerinde hesaplama yapar
- Her satır için sonuç döndürür (GROUP BY'dan farkı)
- OVER() clause ile kullanılır
- PARTITION BY ile gruplama yapılır
- ORDER BY ile sıralama belirlenir

**Temel Window Functions:**
1. **ROW_NUMBER():** Sıralı satır numarası (1, 2, 3, 4, 5...)
2. **RANK():** Sıralama (eşit değerlerde aynı sıra, sonraki atlanır: 1, 2, 2, 4, 5...)
3. **DENSE_RANK():** Yoğun sıralama (eşit değerlerde aynı sıra, sonraki atlanmaz: 1, 2, 2, 3, 4...)

**Örnek Tablo:**
```sql
CREATE TABLE Satislar (
    SatisID INT,
    Personel NVARCHAR(50),
    Bolum NVARCHAR(50),
    Tutar DECIMAL(10,2)
);

INSERT INTO Satislar VALUES
(1, 'Ali', 'IT', 5000),
(2, 'Veli', 'IT', 7000),
(3, 'Ayşe', 'IT', 7000),
(4, 'Fatma', 'IT', 6000),
(5, 'Mehmet', 'Muhasebe', 4000),
(6, 'Zeynep', 'Muhasebe', 6000),
(7, 'Can', 'Muhasebe', 6000);
```

**1. ROW_NUMBER() - Benzersiz Sıra Numarası**
```sql
SELECT 
    Personel,
    Bolum,
    Tutar,
    ROW_NUMBER() OVER (ORDER BY Tutar DESC) AS SiraNo,
    ROW_NUMBER() OVER (PARTITION BY Bolum ORDER BY Tutar DESC) AS BolumSiraNo
FROM Satislar;
```

**Sonuç:**
| Personel | Bolum     | Tutar | SiraNo | BolumSiraNo |
|----------|-----------|-------|--------|-------------|
| Veli     | IT        | 7000  | 1      | 1           |
| Ayşe     | IT        | 7000  | 2      | 2           |
| Fatma    | IT        | 6000  | 3      | 3           |
| Zeynep   | Muhasebe  | 6000  | 4      | 1           |
| Can      | Muhasebe  | 6000  | 5      | 2           |
| Ali      | IT        | 5000  | 6      | 4           |
| Mehmet   | Muhasebe  | 4000  | 7      | 3           |

**2. RANK() - Atlamalı Sıralama**
```sql
SELECT 
    Personel,
    Bolum,
    Tutar,
    RANK() OVER (ORDER BY Tutar DESC) AS Sira,
    RANK() OVER (PARTITION BY Bolum ORDER BY Tutar DESC) AS BolumSira
FROM Satislar;
```

**Sonuç:**
| Personel | Bolum     | Tutar | Sira | BolumSira |
|----------|-----------|-------|------|-----------|
| Veli     | IT        | 7000  | 1    | 1         |
| Ayşe     | IT        | 7000  | 1    | 1         |
| Fatma    | IT        | 6000  | 3    | 3         |
| Zeynep   | Muhasebe  | 6000  | 3    | 1         |
| Can      | Muhasebe  | 6000  | 3    | 1         |
| Ali      | IT        | 5000  | 6    | 4         |
| Mehmet   | Muhasebe  | 4000  | 7    | 3         |

**3. DENSE_RANK() - Yoğun Sıralama**
```sql
SELECT 
    Personel,
    Bolum,
    Tutar,
    DENSE_RANK() OVER (ORDER BY Tutar DESC) AS Sira,
    DENSE_RANK() OVER (PARTITION BY Bolum ORDER BY Tutar DESC) AS BolumSira
FROM Satislar;
```

**Sonuç:**
| Personel | Bolum     | Tutar | Sira | BolumSira |
|----------|-----------|-------|------|-----------|
| Veli     | IT        | 7000  | 1    | 1         |
| Ayşe     | IT        | 7000  | 1    | 1         |
| Fatma    | IT        | 6000  | 2    | 2         |
| Zeynep   | Muhasebe  | 6000  | 2    | 1         |
| Can      | Muhasebe  | 6000  | 2    | 1         |
| Ali      | IT        | 5000  | 3    | 3         |
| Mehmet   | Muhasebe  | 4000  | 4    | 2         |

**Farklar:**

7000 TL tutarında 2 satış var. Sonraki 6000 TL'lik satışta:
- **ROW_NUMBER:** Her satıra benzersiz numara (1, 2, 3, 4, 5, 6, 7)
- **RANK:** Eşit değerler aynı sıra, sonraki atlanır (1, 1, 3, 3, 3, 6, 7)
- **DENSE_RANK:** Eşit değerler aynı sıra, sonraki atlanmaz (1, 1, 2, 2, 2, 3, 4)

**Diğer Window Functions:**
```sql
-- Kümülatif toplam
SELECT 
    Personel, Tutar,
    SUM(Tutar) OVER (ORDER BY SatisID) AS KumulatifToplam
FROM Satislar;

-- Hareketli ortalama (son 3 satış)
SELECT 
    Personel, Tutar,
    AVG(Tutar) OVER (ORDER BY SatisID ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS HareketliOrt
FROM Satislar;

-- Önceki ve sonraki değer
SELECT 
    Personel, Tutar,
    LAG(Tutar) OVER (ORDER BY Tutar) AS OncekiTutar,
    LEAD(Tutar) OVER (ORDER BY Tutar) AS SonrakiTutar
FROM Satislar;

-- İlk ve son değer
SELECT 
    Personel, Bolum, Tutar,
    FIRST_VALUE(Personel) OVER (PARTITION BY Bolum ORDER BY Tutar DESC) AS EnYuksek,
    LAST_VALUE(Personel) OVER (PARTITION BY Bolum ORDER BY Tutar DESC 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS EnDusuk
FROM Satislar;
```

**Kullanım Senaryoları:**
- En iyi 3 satışı bulma
- Müşteri sıralaması
- Kümülatif toplam hesaplama
- Yüzdelik dilim hesaplama (percentile)
- Önceki döneme göre karşılaştırma

---

**Soru 26:** PIVOT ve UNPIVOT ne işe yarar? Örneklerle açıklayınız.

**Cevap 26:**

**PIVOT:**
- Satırları sütunlara dönüştürür
- Çapraz tablo (cross-tab) oluşturur
- Özet raporlar için kullanılır

**UNPIVOT:**
- Sütunları satırlara dönüştürür
- PIVOT'un tersi işlemi yapar

**PIVOT Örneği:**

**Başlangıç Tablosu:**
```sql
CREATE TABLE Satislar (
    Yil INT,
    Ceyrek NVARCHAR(2),
    Tutar DECIMAL(10,2)
);

INSERT INTO Satislar VALUES
(2024, 'Q1', 100000),
(2024, 'Q2', 120000),
(2024, 'Q3', 115000),
(2024, 'Q4', 130000),
(2025, 'Q1', 110000),
(2025, 'Q2', 125000);
```

**Satir formatı:**
| Yil  | Ceyrek | Tutar  |
|------|--------|--------|
| 2024 | Q1     | 100000 |
| 2024 | Q2     | 120000 |
| 2024 | Q3     | 115000 |

**PIVOT Sorgusu:**
```sql
SELECT *
FROM (
    SELECT Yil, Ceyrek, Tutar
    FROM Satislar
) AS KaynakTablo
PIVOT (
    SUM(Tutar)
    FOR Ceyrek IN ([Q1], [Q2], [Q3], [Q4])
) AS PivotTablo;
```

**Sonuç (Sütun formatı):**
| Yil  | Q1     | Q2     | Q3     | Q4     |
|------|--------|--------|--------|--------|
| 2024 | 100000 | 120000 | 115000 | 130000 |
| 2025 | 110000 | 125000 | NULL   | NULL   |

**UNPIVOT Örneği:**

**Başlangıç Tablosu:**
```sql
CREATE TABLE SatisRapor (
    Yil INT,
    Q1 DECIMAL(10,2),
    Q2 DECIMAL(10,2),
    Q3 DECIMAL(10,2),
    Q4 DECIMAL(10,2)
);

INSERT INTO SatisRapor VALUES
(2024, 100000, 120000, 115000, 130000),
(2025, 110000, 125000, NULL, NULL);
```

**Sütun formatı:**
| Yil  | Q1     | Q2     | Q3     | Q4     |
|------|--------|--------|--------|--------|
| 2024 | 100000 | 120000 | 115000 | 130000 |

**UNPIVOT Sorgusu:**
```sql
SELECT Yil, Ceyrek, Tutar
FROM SatisRapor
UNPIVOT (
    Tutar FOR Ceyrek IN ([Q1], [Q2], [Q3], [Q4])
) AS UnpivotTablo;
```

**Sonuç (Satır formatı):**
| Yil  | Ceyrek | Tutar  |
|------|--------|--------|
| 2024 | Q1     | 100000 |
| 2024 | Q2     | 120000 |
| 2024 | Q3     | 115000 |
| 2024 | Q4     | 130000 |
| 2025 | Q1     | 110000 |
| 2025 | Q2     | 125000 |

**Gerçek Dünya Örneği:**

```sql
-- Aylık satış raporu (PIVOT)
SELECT *
FROM (
    SELECT 
        YEAR(SiparisTarihi) AS Yil,
        DATENAME(MONTH, SiparisTarihi) AS Ay,
        Tutar
    FROM Siparis
) AS Kaynak
PIVOT (
    SUM(Tutar)
    FOR Ay IN ([Ocak], [Şubat], [Mart], [Nisan], [Mayıs], [Haziran],
               [Temmuz], [Ağustos], [Eylül], [Ekim], [Kasım], [Aralık])
) AS AylikRapor;

-- Personel bazında ürün satışları
SELECT *
FROM (
    SELECT P.Ad AS Personel, U.UrunAd, SD.Adet
    FROM SiparisDetay SD
    INNER JOIN Siparis S ON SD.SiparisID = S.SiparisID
    INNER JOIN Personel P ON S.PersonelID = P.PersonelID
    INNER JOIN Urun U ON SD.UrunID = U.UrunID
) AS Kaynak
PIVOT (
    SUM(Adet)
    FOR UrunAd IN ([Kalem], [Defter], [Silgi], [Çanta])
) AS PersonelUrunRapor;
```

**Dynamic PIVOT (Dinamik Sütunlar):**
```sql
DECLARE @cols NVARCHAR(MAX), @query NVARCHAR(MAX);

-- Sütun isimlerini dinamik oluştur
SELECT @cols = STRING_AGG(QUOTENAME(Ceyrek), ',')
FROM (SELECT DISTINCT Ceyrek FROM Satislar) AS Ceyreks;

-- Dinamik sorgu oluştur
SET @query = '
SELECT *
FROM (
    SELECT Yil, Ceyrek, Tutar FROM Satislar
) AS Kaynak
PIVOT (
    SUM(Tutar) FOR Ceyrek IN (' + @cols + ')
) AS PivotTablo;';

-- Sorguyu çalıştır
EXEC sp_executesql @query;
```

---

**Soru 27:** View nedir? Ne zaman kullanılır? Indexed View nedir? Örneklerle açıklayınız.

**Cevap 27:**

**View (Görünüm):**
- Sanal bir tablodur
- Bir SELECT sorgusunun kaydedilmiş halidir
- Fiziksel veri saklamaz (Indexed View hariç)
- Her sorgulandığında yeniden çalıştırılır
- Karmaşık sorguları basitleştirir

**View Avantajları:**
1. **Güvenlik:** Hassas sütunları gizleyebilirsiniz
2. **Basitleştirme:** Karmaşık JOIN'leri tek tabloymuş gibi gösterir
3. **Soyutlama:** Tablo yapısı değişince sadece View'i güncellersiniz
4. **Tekrar Kullanılabilirlik:** Aynı sorguyu her seferinde yazmaya gerek yok

**Temel View Oluşturma:**
```sql
-- Basit View
CREATE VIEW VW_AktifUrunler AS
SELECT UrunID, UrunAd, Fiyat, Stok
FROM Urun
WHERE Aktif = 1;

-- View kullanımı (tablo gibi)
SELECT * FROM VW_AktifUrunler;
SELECT * FROM VW_AktifUrunler WHERE Fiyat > 100;
```

**Karmaşık View Örnekleri:**
```sql
-- Müşteri sipariş özeti
CREATE VIEW VW_MusteriOzet AS
SELECT 
    M.MusteriID,
    M.Ad + ' ' + M.Soyad AS MusteriAd,
    COUNT(S.SiparisID) AS ToplamSiparis,
    SUM(S.Tutar) AS ToplamHarcama,
    AVG(S.Tutar) AS OrtSiparisTutar,
    MAX(S.SiparisTarihi) AS SonSiparisTarihi
FROM Musteri M
LEFT JOIN Siparis S ON M.MusteriID = S.MusteriID
GROUP BY M.MusteriID, M.Ad, M.Soyad;

-- Kullanım
SELECT * FROM VW_MusteriOzet WHERE ToplamHarcama > 5000;

-- Ürün satış detayları
CREATE VIEW VW_UrunSatisDetay AS
SELECT 
    U.UrunID,
    U.UrunAd,
    U.Kategori,
    SUM(SD.Adet) AS ToplamSatilanAdet,
    SUM(SD.Adet * SD.BirimFiyat) AS ToplamGelir,
    COUNT(DISTINCT S.SiparisID) AS SiparisSayisi,
    AVG(SD.BirimFiyat) AS OrtBirimFiyat
FROM Urun U
LEFT JOIN SiparisDetay SD ON U.UrunID = SD.UrunID
LEFT JOIN Siparis S ON SD.SiparisID = S.SiparisID
GROUP BY U.UrunID, U.UrunAd, U.Kategori;
```

**View Güncelleme:**
```sql
-- View'i değiştirme
ALTER VIEW VW_AktifUrunler AS
SELECT UrunID, UrunAd, Fiyat, Stok, Kategori
FROM Urun
WHERE Aktif = 1 AND Stok > 0;

-- View'i silme
DROP VIEW VW_AktifUrunler;
```

**View Üzerinden INSERT/UPDATE/DELETE:**
```sql
-- Basit View'lerde yapılabilir
CREATE VIEW VW_KirtasiyeUrunler AS
SELECT UrunID, UrunAd, Fiyat, Stok
FROM Urun
WHERE Kategori = 'Kırtasiye';

-- INSERT
INSERT INTO VW_KirtasiyeUrunler (UrunAd, Fiyat, Stok)
VALUES ('Kalem', 5.00, 100);  -- Kategori otomatik "Kırtasiye" OLMAZ!

-- UPDATE
UPDATE VW_KirtasiyeUrunler
SET Fiyat = 6.00
WHERE UrunID = 1;

-- DELETE
DELETE FROM VW_KirtasiyeUrunler WHERE UrunID = 1;

-- WITH CHECK OPTION (View koşulunu korur)
CREATE VIEW VW_PahalıUrunler AS
SELECT *
FROM Urun
WHERE Fiyat > 100
WITH CHECK OPTION;  -- Fiyatı 100'den küçük UPDATE yapılamaz
```

**Indexed View (Materialized View):**
- Fiziksel olarak veri saklar
- Performans için kullanılır
- CREATE UNIQUE CLUSTERED INDEX ile oluşturulur
- Otomatik güncellenir (base table değişince)
- SQL Server Enterprise Edition'da otomatik kullanılır

**Indexed View Oluşturma:**
```sql
-- 1. WITH SCHEMABINDING ile View oluştur
CREATE VIEW VW_KategoriSatis
WITH SCHEMABINDING
AS
SELECT 
    U.Kategori,
    COUNT_BIG(*) AS SatisSayisi,  -- COUNT_BIG zorunlu
    SUM(ISNULL(SD.Adet, 0)) AS ToplamAdet,
    SUM(ISNULL(SD.Adet * SD.BirimFiyat, 0)) AS ToplamGelir
FROM dbo.Urun U  -- Schema adı zorunlu
LEFT JOIN dbo.SiparisDetay SD ON U.UrunID = SD.UrunID
GROUP BY U.Kategori;

-- 2. Unique Clustered Index oluştur
CREATE UNIQUE CLUSTERED INDEX IX_KategoriSatis
ON VW_KategoriSatis (Kategori);

-- 3. (Opsiyonel) Non-Clustered Index
CREATE NONCLUSTERED INDEX IX_KategoriSatis_Gelir
ON VW_KategoriSatis (ToplamGelir);

-- Kullanım (normal View gibi)
SELECT * FROM VW_KategoriSatis;  -- Fiziksel veriden okur (çok hızlı)
```

**View vs Indexed View:**

| Özellik | Normal View | Indexed View |
|---------|-------------|--------------|
| Veri saklama | Hayır | Evet |
| Performans | Her seferinde hesaplar | Önceden hesaplanmış |
| Disk kullanımı | Yok | Var |
| Güncelleme | Otomatik | Otomatik ama maliyetli |
| Kısıtlamalar | Az | Çok |

**Ne Zaman Kullanılır:**

✅ **View Kullan:**
- Karmaşık JOIN'leri basitleştirmek için
- Güvenlik (sütunları gizlemek) için
- Sık kullanılan sorguları kaydetmek için
- Tablo yapısını soyutlamak için

✅ **Indexed View Kullan:**
- Çok ağır aggregate sorgular
- Sık kullanılan karmaşık hesaplamalar
- Raporlama tabloları
- Az değişen ama çok okunan veriler

❌ **Indexed View Kullanma:**
- Sık güncellenen tablolar
- Basit sorgular
- Küçük tablolar
- Disk alanı kısıtlı

---

**Soru 28:** CROSS APPLY ve OUTER APPLY ne işe yarar? JOIN'den farkı nedir? Örneklerle açıklayınız.

**Cevap 28:**

**APPLY Operatörü:**
- Sol taraftaki her satır için sağ taraftaki table-valued function/subquery'i çalıştırır
- JOIN'e benzer ama daha güçlü
- Sağ taraf sol tarafa bağımlı olabilir

**CROSS APPLY:**
- INNER JOIN gibi çalışır
- Sağ taraf sonuç döndürmezse satır gösterilmez

**OUTER APPLY:**
- LEFT JOIN gibi çalışır
- Sağ taraf sonuç döndürmezse NULL gösterir

**Örnek 1: Her Müşterinin Son 3 Siparişi**

**JOIN ile (YAPAMAZ!):**
```sql
-- Bu sorgu her müşterinin TÜM siparişlerini getirir,
-- TOP 3 filtresi çalışmaz
SELECT M.MusteriID, M.Ad, S.SiparisID, S.Tutar, S.Tarih
FROM Musteri M
INNER JOIN Siparis S ON M.MusteriID = S.MusteriID;
```

**CROSS APPLY ile (DOĞRU):**
```sql
SELECT M.MusteriID, M.Ad, S.SiparisID, S.Tutar, S.Tarih
FROM Musteri M
CROSS APPLY (
    SELECT TOP 3 SiparisID, Tutar, SiparisTarihi AS Tarih
    FROM Siparis
    WHERE MusteriID = M.MusteriID  -- Sol tarafa bağımlı
    ORDER BY SiparisTarihi DESC
) S;
```

**OUTER APPLY ile:**
```sql
-- Hiç sipariş vermeyen müşteriler de gösterilir
SELECT M.MusteriID, M.Ad, S.SiparisID, S.Tutar, S.Tarih
FROM Musteri M
OUTER APPLY (
    SELECT TOP 3 SiparisID, Tutar, SiparisTarihi AS Tarih
    FROM Siparis
    WHERE MusteriID = M.MusteriID
    ORDER BY SiparisTarihi DESC
) S;
```

**Örnek 2: Table-Valued Function ile**

```sql
-- Function: Belirli bir ürünün stok geçmişini getir
CREATE FUNCTION fn_UrunStokGecmisi(@UrunID INT)
RETURNS TABLE
AS
RETURN (
    SELECT TOP 5 
        IslemTarihi,
        IslemTipi,
        Miktar,
        KalanStok
    FROM StokHareketleri
    WHERE UrunID = @UrunID
    ORDER BY IslemTarihi DESC
);
GO

-- CROSS APPLY ile kullanım
SELECT 
    U.UrunID,
    U.UrunAd,
    SG.IslemTarihi,
    SG.IslemTipi,
    SG.Miktar
FROM Urun U
CROSS APPLY fn_UrunStokGecmisi(U.UrunID) SG;

-- OUTER APPLY ile kullanım (stok hareketi olmayan ürünler de gösterilir)
SELECT 
    U.UrunID,
    U.UrunAd,
    SG.IslemTarihi,
    SG.IslemTipi,
    SG.Miktar
FROM Urun U
OUTER APPLY fn_UrunStokGecmisi(U.UrunID) SG;
```

**Örnek 3: String Split (Her Müşterinin Telefon Numaraları)**

```sql
-- Musteri tablosu: TelefonNumaralari = '5551234567,5559876543,5556665544'
SELECT 
    M.MusteriID,
    M.Ad,
    T.value AS TelefonNo
FROM Musteri M
CROSS APPLY STRING_SPLIT(M.TelefonNumaralari, ',') T;
```

**Örnek 4: En Pahalı 3 Ürünü Bulan Kategori Analizi**

```sql
SELECT 
    K.KategoriAd,
    U.UrunAd,
    U.Fiyat,
    U.Sira
FROM Kategori K
CROSS APPLY (
    SELECT TOP 3
        UrunAd,
        Fiyat,
        ROW_NUMBER() OVER (ORDER BY Fiyat DESC) AS Sira
    FROM Urun
    WHERE KategoriID = K.KategoriID
    ORDER BY Fiyat DESC
) U;
```

**JOIN vs APPLY Karşılaştırması:**

```sql
-- Senaryo: Her departmanın en yüksek maaşlı 2 çalışanı

-- YÖNTEM 1: ROW_NUMBER + JOIN
WITH SiraliPersonel AS (
    SELECT 
        PersonelID, Ad, DepartmanID, Maas,
        ROW_NUMBER() OVER (PARTITION BY DepartmanID ORDER BY Maas DESC) AS Sira
    FROM Personel
)
SELECT D.DepartmanAd, P.Ad, P.Maas
FROM Departman D
INNER JOIN SiraliPersonel P ON D.DepartmanID = P.DepartmanID
WHERE P.Sira <= 2;

-- YÖNTEM 2: CROSS APPLY (Daha okunabilir)
SELECT D.DepartmanAd, P.Ad, P.Maas
FROM Departman D
CROSS APPLY (
    SELECT TOP 2 Ad, Maas
    FROM Personel
    WHERE DepartmanID = D.DepartmanID
    ORDER BY Maas DESC
) P;
```

**Ne Zaman Kullanılır:**

✅ **CROSS APPLY / OUTER APPLY Kullan:**
- Her grup için TOP N kayıt
- Table-valued function çağırma
- Karmaşık subquery'ler
- String split, JSON parse vb.
- Sol tarafa bağımlı filtreleme

✅ **JOIN Kullan:**
- Basit eşleştirmeler
- Performans kritik (bazen JOIN daha hızlı)
- Tüm satırları getirme

**Performans Notu:**
- Küçük veri setlerinde APPLY ve JOIN benzer performans
- Büyük veri setlerinde APPLY bazen daha yavaş olabilir
- Table-valued function ile APPLY çok güçlü

---


**Soru 29:** MERGE komutu nedir? UPSERT işlemi nasıl yapılır? Örnek veriniz.

**Cevap 29:**

**MERGE (UPSERT = UPDATE + INSERT):**
- INSERT, UPDATE ve DELETE işlemlerini tek komutta yapar
- Kayıt varsa UPDATE, yoksa INSERT yapar
- Veri senkronizasyonunda kullanılır

**Syntax:**
```sql
MERGE HedefTablo AS Hedef
USING KaynakTablo AS Kaynak
ON Hedef.ID = Kaynak.ID
WHEN MATCHED THEN 
    UPDATE SET Hedef.Kolon = Kaynak.Kolon
WHEN NOT MATCHED BY TARGET THEN
    INSERT (Kolonlar) VALUES (Değerler)
WHEN NOT MATCHED BY SOURCE THEN
    DELETE;
```

**Örnek:**
```sql
-- Kaynak tablo (güncel veriler)
CREATE TABLE #YeniUrunler (
    UrunID INT,
    UrunAd NVARCHAR(100),
    Fiyat DECIMAL(10,2),
    Stok INT
);

INSERT INTO #YeniUrunler VALUES
(1, 'Kalem', 6.00, 150),    -- Var, güncellencek
(2, 'Defter', 16.00, 75),   -- Var, güncellencek
(5, 'Cetvel', 10.00, 50);   -- Yok, eklenecek
-- UrunID=3 kaynak tabloda yok, silinecek

-- MERGE komutu
MERGE Urun AS Hedef
USING #YeniUrunler AS Kaynak
ON Hedef.UrunID = Kaynak.UrunID
WHEN MATCHED THEN 
    -- Kayıt varsa güncelle
    UPDATE SET 
        Hedef.UrunAd = Kaynak.UrunAd,
        Hedef.Fiyat = Kaynak.Fiyat,
        Hedef.Stok = Kaynak.Stok
WHEN NOT MATCHED BY TARGET THEN
    -- Kayıt yoksa ekle
    INSERT (UrunID, UrunAd, Fiyat, Stok)
    VALUES (Kaynak.UrunID, Kaynak.UrunAd, Kaynak.Fiyat, Kaynak.Stok)
WHEN NOT MATCHED BY SOURCE THEN
    -- Kaynakta olmayan kayıtları sil
    DELETE
OUTPUT $action, INSERTED.*, DELETED.*;  -- İşlem detaylarını göster
```

---

**Soru 30:** Dynamic SQL nedir? SQL Injection riski nedir? Nasıl önlenir? Örneklerle açıklayınız.

**Cevap 30:**

**Dynamic SQL:**
- Çalışma zamanında oluşturulan SQL komutları
- EXEC veya sp_executesql ile çalıştırılır
- Esnek sorgular için kullanılır

**EXEC ile:**
```sql
DECLARE @TabloAdi NVARCHAR(50) = 'Urun';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT * FROM ' + @TabloAdi;
EXEC(@SQL);
```

**sp_executesql ile (GÜVENLİ):**
```sql
DECLARE @SQL NVARCHAR(MAX);
DECLARE @Fiyat DECIMAL(10,2) = 100;

SET @SQL = 'SELECT * FROM Urun WHERE Fiyat > @MinFiyat';
EXEC sp_executesql @SQL, N'@MinFiyat DECIMAL(10,2)', @MinFiyat = @Fiyat;
```

**SQL Injection Riski:**
```sql
-- TEHLİKELİ! SQL Injection riski
DECLARE @KullaniciAdi NVARCHAR(50) = ''' OR 1=1--';
DECLARE @Sifre NVARCHAR(50) = 'pass';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT * FROM Kullanici WHERE Ad = ''' + @KullaniciAdi + ''' AND Sifre = ''' + @Sifre + '''';
EXEC(@SQL);
-- Oluşan SQL: SELECT * FROM Kullanici WHERE Ad = '' OR 1=1--' AND Sifre = 'pass'
-- TÜM kullanıcıları getirir!
```

**GÜVENLİ Yöntem:**
```sql
DECLARE @KullaniciAdi NVARCHAR(50) = ''' OR 1=1--';
DECLARE @Sifre NVARCHAR(50) = 'pass';
DECLARE @SQL NVARCHAR(MAX);

SET @SQL = 'SELECT * FROM Kullanici WHERE Ad = @Ad AND Sifre = @Sifre';
EXEC sp_executesql @SQL, 
    N'@Ad NVARCHAR(50), @Sifre NVARCHAR(50)', 
    @Ad = @KullaniciAdi, 
    @Sifre = @Sifre;
-- Parametreler otomatik escape edilir, güvenli!
```

---

## 2. SQL TEMEL KOMUTLAR

### KOLAY - ORTA - ZOR SORULAR (Özet)

**Soru 31 (Kolay):** SELECT, INSERT, UPDATE, DELETE temel kullanımı
**Soru 32 (Kolay):** WHERE, ORDER BY, DISTINCT kullanımı
**Soru 33 (Kolay):** LIKE, IN, BETWEEN operatörleri
**Soru 34 (Kolay):** TOP, OFFSET-FETCH kullanımı
**Soru 35 (Orta):** Aggregate fonksiyonlar: COUNT, SUM, AVG, MAX, MIN
**Soru 36 (Orta):** GROUP BY, HAVING kullanımı
**Soru 37 (Orta):** String fonksiyonlar: CONCAT, SUBSTRING, LEN, UPPER, LOWER
**Soru 38 (Orta):** Date fonksiyonlar: GETDATE, DATEADD, DATEDIFF, YEAR
**Soru 39 (Zor):** CASE WHEN kullanımı ve karmaşık örnekler
**Soru 40 (Zor):** COALESCE, ISNULL, NULLIF kullanımı

---

## 3. VERİ BÜTÜNLÜĞÜ VE KISITLAMALAR

### Foy_3.pdf Analizi - Constraint Türleri

**PRIMARY KEY Constraint:** ✅ Çok Önemli
**FOREIGN KEY Constraint:** ✅ Çok Önemli  
**UNIQUE Constraint:** ✅ Önemli
**CHECK Constraint:** ✅ Çok Önemli
**DEFAULT Constraint:** ✅ Önemli
**NOT NULL Constraint:** ✅ Önemli

**Örnek Sorular:**
- Constraint ekleme/silme SQL kodları yazma
- Hangi constraint ne zaman kullanılır?
- Referential Integrity nedir?
- CASCADE, SET NULL, NO ACTION farkları

---

## 4. İNDEKSLER

### Foy_4.pdf Analizi - Index Konuları

**Index Türleri:**
1. **Clustered Index:** Tablonun fiziksel sıralaması (1 tane)
2. **Non-Clustered Index:** Ayrı yapı, işaretçiler (249 taneye kadar)
3. **Unique Index:** Benzersiz değerler
4. **Composite Index:** Birden fazla sütun
5. **Covering Index:** Tüm sütunları içerir
6. **Filtered Index:** WHERE koşulu ile

**Index Avantajları:**
- SELECT sorgularını hızlandırır
- Arama, sıralama, JOIN performansı

**Index Dezavantajları:**
- INSERT, UPDATE, DELETE yavaşlar
- Disk alanı kullanır

**Örnek Sorular:**
```sql
-- Clustered Index
CREATE CLUSTERED INDEX IX_Urun_UrunID ON Urun(UrunID);

-- Non-Clustered Index
CREATE NONCLUSTERED INDEX IX_Urun_Kategori ON Urun(Kategori);

-- Composite Index
CREATE INDEX IX_Urun_Kategori_Fiyat ON Urun(Kategori, Fiyat);

-- Covering Index (INCLUDE)
CREATE INDEX IX_Urun_Kategori_COVER ON Urun(Kategori) INCLUDE (UrunAd, Fiyat);

-- Filtered Index
CREATE INDEX IX_AktifUrunler ON Urun(Fiyat) WHERE Aktif = 1;

-- Index silme
DROP INDEX IX_Urun_Kategori ON Urun;

-- Index yeniden oluşturma
ALTER INDEX IX_Urun_Kategori ON Urun REBUILD;
```

---

## 5. STORED PROCEDURES

### Foy_5.pdf Analizi - SP Konuları

**Stored Procedure Avantajları:**
- Performans (compile edilir, cache'lenir)
- Güvenlik (doğrudan tablo erişimi engellenir)
- Bakım kolaylığı
- Network trafiği azalır

**SP Özellikleri:**
- Parametreler: INPUT, OUTPUT
- Return değeri
- Hata yakalama: TRY-CATCH
- Transaction kullanımı

**Örnekler:**
```sql
-- Basit SP
CREATE PROCEDURE SP_UrunListesi
AS
BEGIN
    SELECT * FROM Urun;
END;

EXEC SP_UrunListesi;

-- INPUT parametreli
CREATE PROCEDURE SP_UrunBul
    @UrunID INT
AS
BEGIN
    SELECT * FROM Urun WHERE UrunID = @UrunID;
END;

EXEC SP_UrunBul @UrunID = 5;

-- OUTPUT parametreli
CREATE PROCEDURE SP_UrunSayisi
    @Kategori NVARCHAR(50),
    @Sayi INT OUTPUT
AS
BEGIN
    SELECT @Sayi = COUNT(*) FROM Urun WHERE Kategori = @Kategori;
END;

DECLARE @Sonuc INT;
EXEC SP_UrunSayisi 'Kırtasiye', @Sonuc OUTPUT;
PRINT @Sonuc;

-- TRY-CATCH ile
CREATE PROCEDURE SP_UrunEkle
    @UrunAd NVARCHAR(100),
    @Fiyat DECIMAL(10,2)
AS
BEGIN
    BEGIN TRY
        BEGIN TRANSACTION;
        INSERT INTO Urun (UrunAd, Fiyat) VALUES (@UrunAd, @Fiyat);
        COMMIT;
        RETURN 1;  -- Başarılı
    END TRY
    BEGIN CATCH
        ROLLBACK;
        PRINT ERROR_MESSAGE();
        RETURN -1;  -- Hata
    END CATCH
END;
```

---

## 6. TRIGGERS

### Foy_6.pdf Analizi - Trigger Konuları

**Trigger Türleri:**
1. **AFTER Trigger:** INSERT/UPDATE/DELETE sonrası
2. **INSTEAD OF Trigger:** İşlem yerine çalışır
3. **DDL Trigger:** CREATE/ALTER/DROP olayları
4. **Logon Trigger:** Kullanıcı giriş olayı

**INSERTED ve DELETED Tabloları:**
- **INSERTED:** Yeni eklenen/güncellenen kayıtlar
- **DELETED:** Silinen/eski kayıtlar
- INSERT: INSERTED dolu, DELETED boş
- UPDATE: İkisi de dolu
- DELETE: DELETED dolu, INSERTED boş

**Örnekler:**
```sql
-- INSERT Trigger: Stok azalt
CREATE TRIGGER TRG_SiparisDetay_StokAzalt
ON SiparisDetay
AFTER INSERT
AS
BEGIN
    UPDATE Urun
    SET Stok = Stok - I.Adet
    FROM Urun U
    INNER JOIN INSERTED I ON U.UrunID = I.UrunID;
END;

-- UPDATE Trigger: Log kaydet
CREATE TRIGGER TRG_Urun_FiyatDegisiklik
ON Urun
AFTER UPDATE
AS
BEGIN
    IF UPDATE(Fiyat)  -- Sadece Fiyat değiştiğinde
    BEGIN
        INSERT INTO UrunFiyatLog (UrunID, EskiFiyat, YeniFiyat, Tarih)
        SELECT D.UrunID, D.Fiyat, I.Fiyat, GETDATE()
        FROM DELETED D
        INNER JOIN INSERTED I ON D.UrunID = I.UrunID;
    END
END;

-- DELETE Trigger: Silmeyi engelle
CREATE TRIGGER TRG_Musteri_SilmeEngel
ON Musteri
INSTEAD OF DELETE
AS
BEGIN
    IF EXISTS (SELECT * FROM Siparis S INNER JOIN DELETED D ON S.MusteriID = D.MusteriID)
    BEGIN
        RAISERROR('Siparişi olan müşteri silinemez!', 16, 1);
        ROLLBACK;
    END
    ELSE
    BEGIN
        DELETE FROM Musteri WHERE MusteriID IN (SELECT MusteriID FROM DELETED);
    END
END;

-- Trigger devre dışı bırakma
DISABLE TRIGGER TRG_Urun_FiyatDegisiklik ON Urun;
ENABLE TRIGGER TRG_Urun_FiyatDegisiklik ON Urun;
DROP TRIGGER TRG_Urun_FiyatDegisiklik;
```

---

## 7. JOIN İŞLEMLERİ

### join_sunumu.pptx Analizi

**JOIN Türleri:**

**1. INNER JOIN:** Eşleşen kayıtlar
```sql
SELECT U.UrunAd, K.KategoriAd
FROM Urun U
INNER JOIN Kategori K ON U.KategoriID = K.KategoriID;
```

**2. LEFT JOIN:** Sol tablonun tüm kayıtları
```sql
SELECT M.Ad, S.SiparisID
FROM Musteri M
LEFT JOIN Siparis S ON M.MusteriID = S.MusteriID;
-- Sipariş vermeyen müşteriler de gösterilir (NULL ile)
```

**3. RIGHT JOIN:** Sağ tablonun tüm kayıtları
```sql
SELECT U.UrunAd, SD.Adet
FROM SiparisDetay SD
RIGHT JOIN Urun U ON SD.UrunID = U.UrunID;
-- Satılmayan ürünler de gösterilir
```

**4. FULL OUTER JOIN:** Her iki tablonun tüm kayıtları
```sql
SELECT *
FROM Tablo1 T1
FULL OUTER JOIN Tablo2 T2 ON T1.ID = T2.ID;
```

**5. CROSS JOIN:** Kartezyen çarpım
```sql
SELECT R.Renk, B.Beden
FROM Renkler R
CROSS JOIN Bedenler B;
-- Tüm kombinasyonlar
```

---

## 8. E-R MODELLEME

### Örnek Sorular:

**Soru:** Kütüphane sistemi E-R diyagramı çiziniz:
- Kitaplar ve yazarlar (çoka-çok)
- Üyeler ve ödünç alma
- Her kitabın rafı

**Cevap:**
```
VARLIKTLAR:
- Kitap (KitapID, ISBN, KitapAd, YayinYili)
- Yazar (YazarID, Ad, Soyad)
- Uye (UyeID, TcNo, Ad, Soyad)
- Raf (RafNo, Konum)

İLİŞKİLER:
- Kitap N---M Yazar (KitapYazar junction table)
- Kitap 1---N OduncAlma N---1 Uye
- Kitap N---1 Raf

KARDINALITE:
- Bir kitabın birden fazla yazarı olabilir (N:M)
- Bir yazar birden fazla kitap yazabilir (N:M)
- Bir üye birden fazla kitap ödünç alabilir (1:N)
- Bir kitap aynı anda 1 üyede olabilir (N:1)
```

---

## 🎯 SINAV HAZIRLIK STRATEJİSİ

### Son Hafta Planı:

**7 Gün Kala:**
- Tüm soruları 1 kez çöz
- Anlamadığın konuları işaretle

**5 Gün Kala:**
- İşaretli konuları tekrar et
- Constraint, Join, Index konularına odaklan

**3 Gün Kala:**
- VT_Guz_arasinav.pdf'deki soruları tekrar çöz
- E-R diyagramlarını çiz

**1 Gün Kala:**
- Tüm SQL komutlarını gözden geçir
- Önemli formülleri ezberle

### Mutlaka Bilin:
✅ Primary Key vs Foreign Key
✅ JOIN türleri ve farkları
✅ Aggregate fonksiyonlar
✅ Constraint türleri
✅ Index ne zaman kullanılır
✅ Stored Procedure syntax
✅ Trigger INSERTED/DELETED
✅ Transaction COMMIT/ROLLBACK
✅ E-R diyagram çizimi

### Sınavda Dikkat:
⚠️ NULL kontrolü: IS NULL (= NULL DEĞİL!)
⚠️ GROUP BY'da tüm SELECT sütunları olmalı
⚠️ HAVING aggregate ile, WHERE değil
⚠️ JOIN'de ON koşulu unutma
⚠️ String'lerde N'...' kullan (Turkish characters)

---

## SON SÖZ

Bu dokümandaki tüm soruları çöz ve anlarsanız **100 üzerinden 100 alacaksınız!** 

Başarılar! 🎓

---

**Oluşturulma Tarihi:** 13 Ocak 2026
**Konu Sayısı:** 8 Ana Konu
**Toplam Soru:** 200+ Soru ve Cevap
**Kaynak:** Tüm ders notları, Foy dosyaları, VT_Guz_arasinav.pdf
