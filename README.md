andır, tablolar arası ilişki kurar.

### 1.3 SQL Alt Dil Grupları
SQL dili, işlevlerine göre alt gruplara ayrılır:

1. **DDL (Data Definition Language - Veri Tanımlama Dili)**
   - Veri tabanı yapısını tanımlar
   - Komutlar: CREATE, ALTER, DROP, TRUNCATE
   - Örnek: `CREATE TABLE ogrenciler (ogrenci_no INT PRIMARY KEY)`

2. **DML (Data Manipulation Language - Veri İşleme Dili)**
   - Verileri ekleme, güncelleme, silme işlemleri
   - Komutlar: INSERT, UPDATE, DELETE, SELECT
   - Örnek: `INSERT INTO ogrenciler VALUES (101, 'Ahmet')`

3. **DCL (Data Control Language - Veri Kontrol Dili)**
   - Kullanıcı yetkilendirme işlemleri
   - Komutlar: GRANT, REVOKE
   - Örnek: `GRANT SELECT ON ogrenciler TO kullanici1`

4. **TCL (Transaction Control Language - İşlem Kontrol Dili)**
   - Transaction yönetimi
   - Komutlar: COMMIT, ROLLBACK, SAVEPOINT

### 1.4 VTYS'nin Dosyalama Sistemlerine Göre Üstünlükleri
- **Veri Bütünlüğü**: Primary key, foreign key gibi kısıtlamalarla veri tutarlılığı sağlanır
- **Veri Tekrarının Azaltılması**: Normalizasyon ile gereksiz veri tekrarı önlenir
- **Eşzamanlı Erişim**: Birden fazla kullanıcı aynı anda veriye erişebilir
- **Veri Güvenliği**: Kullanıcı yetkilendirme sistemi ile güvenlik sağlanır
- **Yedekleme ve Kurtarma**: Otomatik yedekleme ve veri kurtarma mekanizmaları vardır
- **Sorgulama Kolaylığı**: SQL ile karmaşık sorgular kolayca yapılabilir
- **Veri Bağımsızlığı**: Fiziksel ve mantıksal veri bağımsızlığı sağlanır

---

## 2. E-R (VARLIK-İLİŞKİ) DİYAGRAMI

### 2.1 E-R Diyagramı Nedir?
Varlık-İlişki diyagramı, gerçek hayat senaryolarını veri tabanı modeline dönüştürmek için kullanılan görsel modeldir. Varlıkları (entity) ve aralarındaki ilişkileri gösterir.

### 2.2 İlişki Türleri
- **1:1 (Bire-Bir)**: Bir varlık diğer varlıkla sadece bir kez ilişkilidir
- **1:N (Bire-Çok)**: Bir varlık diğer varlıkla birden fazla kez ilişkilidir
- **N:M (Çoka-Çok)**: Her iki varlık da birden fazla kez ilişkilidir (ara tablo gerektirir)

### 2.3 Tablo Dönüşüm Kuralları
- **1:1 ilişki**: Bir, iki veya üç tablo olabilir. Zorunlu olmayan tarafın anahtarı, zorunlu tarafa alan olarak eklenir.
- **1:N ilişki**: İki veya üç tablo. 1 derecelinin (parent) anahtarı, N dereceliye (child) alan olarak eklenir.
- **N:M ilişki**: Üç tablo gerekir (iki ana tablo + bir ara tablo). Ara tabloda her iki tablonun primary key'leri foreign key olarak bulunur.

### 2.4 Örnek Senaryolar

#### ÖRNEK 1: Okul Yönetim Sistemi
**Kurallar:**
1. Öğretmenler bölümlerde görev yapar. Bir bölümde birçok öğretmen olabilir ama bir öğretmen tek bir bölümde çalışır. **(1:N)**
2. Öğrenciler derslere kayıt olur. Bir öğrenci birçok ders alabilir, bir dersi birçok öğrenci alabilir. **(N:M)**
3. Dersler öğretmenler tarafından verilir. Bir öğretmen birçok derse girebilir. **(1:N)**
4. Her dersin bir sınıfı vardır, bir sınıfta birden fazla ders olabilir. **(1:N)**

**Tablolar:**
- Bolumler(bolum_id, bolumAdi)
- Ogretmenler(ogretmen_id, adSoyad, bolum_id)
- Dersler(ders_id, dersAdi, ogretmen_id, sinif_id)
- Ogrenciler(ogrenci_id, adSoyad)
- Siniflar(sinif_id, sinifAdi)
- OgrenciDers(ogrenci_id, ders_id) ← N:M ara tablo

#### ÖRNEK 2: Kargo Takip Sistemi
**Kurallar:**
1. Müşteriler gönderi oluşturur. Bir müşteri birçok gönderi oluşturabilir. **(1:N)**
2. Her gönderi bir şubeden teslim edilir. Bir şube birçok gönderiyi işleyebilir. **(1:N)**
3. Gönderi bir kargo personeline atanır. Bir personel birçok gönderi taşıyabilir ama bir gönderi sadece bir personele atanır. **(1:N)**
4. Gönderi durumları (KargoDurum) ayrı tabloda tutulur, gönderiyle birebir ilişkilidir. **(1:1)**

**Tablolar:**
- Musteriler(musteri_id, adSoyad, telefon)
- Subeler(sube_id, subeAdi, adres)
- Personeller(personel_id, adSoyad)
- Gonderiler(gonderi_id, musteri_id, sube_id, personel_id, tarih)
- KargoDurum(durum_id, gonderi_id, durum, tarih)

#### ÖRNEK 3: Kütüphane Otomasyonu
**Kurallar:**
1. Kitaplar yazarlara aittir. Bir yazar birçok kitap yazabilir. **(1:N)**
2. Üyeler kitap ödünç alabilir. Bir üye birçok kitap alabilir, bir kitap zaman içinde birçok üyeye verilmiş olabilir. **(N:M)**
3. Ödünç alma işlemleri personel tarafından yapılır. **(1:N)**
4. Kitaplar bir kategoriye aittir, bir kategori birçok kitabı kapsar. **(1:N)**

**Tablolar:**
- Yazarlar(yazar_id, adSoyad)
- Kategoriler(kategori_id, kategoriAdi)
- Kitaplar(kitap_id, kitapAdi, yazar_id, kategori_id)
- Uyeler(uye_id, adSoyad, telefon)
- Personeller(personel_id, adSoyad)
- Odunc(odunc_id, uye_id, kitap_id, personel_id, alisTarihi, iadeTarihi)

---

## 3. SQL İŞLEMLERİ

### 3.1 CREATE DATABASE (Veri Tabanı Oluşturma)

```sql
CREATE DATABASE ogrenci
ON PRIMARY
(
    NAME = ogrenci_veri,
    FILENAME = 'D:\data\ogrenci.mdf',
    SIZE = 10 MB,
    MAXSIZE = 100 MB,
    FILEGROWTH = 25%
)
LOG ON
(
    NAME = o_veri_log,
    FILENAME = 'D:\data\ogrenci.ldf',
    SIZE = 5 MB,
    MAXSIZE = 50 MB,
    FILEGROWTH = 25%
);
```

**Parametreler:**
- **NAME**: Dosyanın mantıksal adı
- **FILENAME**: Fiziksel dosya yolu
- **SIZE**: Başlangıç boyutu
- **MAXSIZE**: Maksimum boyut
- **FILEGROWTH**: Büyüme oranı (% veya MB)

### 3.2 CREATE TABLE (Tablo Oluşturma)

```sql
CREATE TABLE tblogrenci
(
    ogrenci_no INT IDENTITY(1000,1) PRIMARY KEY,
    tckimlikno NCHAR(11) NOT NULL,
    ad NVARCHAR(20),
    soyad NVARCHAR(20)
);
```

**Veri Tipleri:**
- **INT**: Tam sayı
- **CHAR(n)**: Sabit uzunlukta metin
- **VARCHAR(n)**: Değişken uzunlukta metin
- **NCHAR(n)**: Unicode sabit uzunlukta metin
- **NVARCHAR(n)**: Unicode değişken uzunlukta metin
- **DATETIME**: Tarih ve saat
- **IDENTITY(başlangıç, artış)**: Otomatik artan sayı

### 3.3 ALTER TABLE (Tablo Değiştirme)

```sql
-- Yeni alan ekleme
ALTER TABLE tablo_adi ADD alan_adi alan_turu;

-- Örnek
ALTER TABLE tblogrenci ADD dogumTarihi DATETIME;
```

### 3.4 INSERT (Veri Ekleme)

```sql
-- Tek satır ekleme
INSERT INTO tbl_ogrenci (ogrNo, adSoyad, bolum_id)
VALUES (101, 'Mert Yıldırım', 1);

-- Çoklu satır ekleme
INSERT INTO tbl_ogrenci (ogrNo, adSoyad, bolum_id)
VALUES
    (301239, 'Musa Aslan', 1),
    (301240, 'İbrahim Uğur Yılmaz', 2),
    (301241, 'Mustafa Topsakal', 3);
```

### 3.5 SELECT Sorguları

#### Temel SELECT
```sql
SELECT urunad, listefiyat FROM tblurun;
```

#### DISTINCT (Tekil Değerler)
```sql
SELECT DISTINCT marka FROM tblurun ORDER BY marka DESC;
```
**Sonuç:** Tekrar etmeyen marka isimleri, Z'den A'ya sıralı

#### WHERE (Filtreleme)
```sql
SELECT urunad, listefiyat FROM tblurun
WHERE marka = 'Microsoft';
```

#### LIKE (Desen Eşleştirme)
```sql
SELECT urunad, listefiyat FROM tblurun
WHERE marka LIKE 'M%';
```
**% işareti:** M ile başlayan tüm markalar

#### ORDER BY (Sıralama)
```sql
SELECT urunad, listefiyat FROM tblurun
ORDER BY listefiyat ASC;  -- Artan sıralama
```

#### TOP (İlk N Kayıt)
```sql
SELECT TOP(3) urunad, listefiyat FROM tblurun
ORDER BY listefiyat;
```
**Sonuç:** En düşük fiyatlı 3 ürün

#### Subquery (Alt Sorgu)
```sql
SELECT urunad, listefiyat, listefiyat*1.1 FROM tblurun
WHERE marka = (SELECT marka FROM tblurun WHERE urunad='Bilgisayar');
```
**Açıklama:** Önce bilgisayarın markası bulunur, sonra o markanın tüm ürünleri listelenir

#### Hesaplamalar
```sql
SELECT urunad, listefiyat, listefiyat*1.1 AS yeniFiyat FROM tblurun;
```

---

## 4. JOIN İŞLEMLERİ

### 4.1 JOIN Nedir?
JOIN, iki veya daha fazla tabloyu ortak alan üzerinden birleştirmeye yarar.

### 4.2 RIGHT JOIN

```sql
SELECT b.bolumAdi, o.adSoyad
FROM tbl_ogrenci o
RIGHT JOIN tbl_bolum b
ON o.bolum_id = b.bolum_id;
```

**Örnek Tablolar:**

**tbl_ogrenci:**
| ogrNo | adSoyad | bolum_id |
|-------|---------|----------|
| 101 | Mert Yıldırım | 1 |
| 102 | Elif Güneş | 2 |
| 103 | Hasan Çetin | NULL |
| 104 | Derya Polat | 4 |
| 105 | Buse Korkmaz | 5 |

**tbl_bolum:**
| bolum_id | bolumAdi |
|----------|----------|
| 1 | Yazılım |
| 2 | Elektrik |
| 3 | Makine |
| 6 | Endüstri |

**Sonuç:**
| bolumAdi | adSoyad |
|----------|---------|
| Yazılım | Mert Yıldırım |
| Elektrik | Elif Güneş |
| Makine | NULL |
| Endüstri | NULL |

**Açıklama:** RIGHT JOIN'de SAĞ tablodaki (tbl_bolum) TÜM satırlar gelir. Öğrencisi olmayan bölümler için adSoyad = NULL olur.

### 4.3 Diğer JOIN Türleri
- **INNER JOIN**: Her iki tabloda da eşleşen kayıtlar
- **LEFT JOIN**: Sol tablodaki tüm kayıtlar + sağdan eşleşenler
- **RIGHT JOIN**: Sağ tablodaki tüm kayıtlar + soldan eşleşenler
- **FULL OUTER JOIN**: Her iki tablodaki tüm kayıtlar

---

## 5. STORED PROCEDURE (SAKLANAN YORDAM)

### 5.1 Stored Procedure Nedir?
Stored Procedure, veri tabanında saklanan ve tekrar tekrar çalıştırılabilen SQL kod bloklarıdır. Karmaşık işlemleri tek bir komutla çalıştırmaya yarar.

### 5.2 Avantajları
- Performans artışı (önceden derlenmiş kod)
- Kod tekrarını azaltır
- Güvenlik (kullanıcılar sadece SP'yi çalıştırır, tablolara direkt erişmez)
- Merkezi yönetim (değişiklik tek yerden yapılır)

### 5.3 Örnek: Haftalık Ödünç/İade Raporu

```sql
CREATE PROCEDURE sp_HaftalikRapor
AS
BEGIN
    SELECT
        o.OduncID,
        ogr.AdSoyad,
        kt.KitapAdi,
        o.AlisTarihi,
        o.IadeTarihi,
        CASE
            WHEN o.IadeDurumu = 1 THEN 'İade Edildi'
            ELSE 'Teslim Bekleniyor'
        END AS Durum
    FROM Odunc o
    INNER JOIN Ogrenciler ogr ON o.OgrenciID = ogr.OgrenciID
    INNER JOIN Kitaplar kt ON o.KitapID = kt.KitapID
    WHERE o.AlisTarihi >= DATEADD(DAY, -7, GETDATE());
END;
```

**Çağırma:**
```sql
EXEC sp_HaftalikRapor;
```

**Backend Entegrasyonu (Spring Boot):**
```java
@Procedure("sp_HaftalikRapor")
List<Rapor> getWeeklyReport();
```

### 5.4 Diğer Örnekler
- **bugunkiSiparisler**: Bugünkü siparişleri listeler
- **Nesting Kullanımı**: Bir SP içinde başka SP çağrılabilir

### 5.5 Zamanlanmış Görevler
Stored Procedure'ler otomatik olarak çalıştırılabilir:
- **Hangfire**: .NET için zamanlama kütüphanesi
- **Quartz.NET**: .NET için gelişmiş zamanlama
- **Cronjob**: Linux tabanlı zamanlama sistemi

---

## 6. TRIGGER (TETİKLEYİCİ)

### 6.1 Trigger Nedir?
Trigger, bir tabloda INSERT, UPDATE veya DELETE işlemi gerçekleştiğinde otomatik olarak çalışan SQL kod bloklarıdır. Veri bütünlüğünü korumak ve otomatik işlemler yapmak için kullanılır.

### 6.2 Trigger Türleri
- **AFTER Trigger**: İşlem tamamlandıktan SONRA çalışır
- **INSTEAD OF Trigger**: İşlem YERINE çalışır
- **INSERT Trigger**: Veri eklendiğinde çalışır
- **UPDATE Trigger**: Veri güncellendiğinde çalışır
- **DELETE Trigger**: Veri silindiğinde çalışır

### 6.3 Özel Tablolar
- **inserted**: Eklenen veya güncellenen YENİ verileri tutar
- **deleted**: Silinen veya güncellenenden ÖNCEKİ verileri tutar

### 6.4 Örnek: Geç İade Ceza Hesaplama

**Senaryo:** Öğrenci kitabı geç iade ederse sistem otomatik olarak ceza oluşturacak. 7 günü aşan her gün için 5 TL ceza kesilir.

**Tablolar:**
- **Odunc**: (OduncID, OgrenciID, KitapID, AlisTarihi, IadeTarihi, IadeDurumu)
- **CezaKayitlari**: (CezaID, OgrenciID, OduncID, GecikmeGun, CezaMiktari, Tarih)

```sql
CREATE TRIGGER trg_CezaHesapla
ON Odunc
AFTER UPDATE
AS
BEGIN
    DECLARE @OduncID INT,
            @OgrenciID INT,
            @AlisTarihi DATE,
            @IadeTarihi DATE,
            @Gecikme INT;

    -- Güncellenen kaydın bilgilerini al
    SELECT
        @OduncID = OduncID,
        @OgrenciID = OgrenciID,
        @AlisTarihi = AlisTarihi,
        @IadeTarihi = IadeTarihi
    FROM inserted;

    -- Gecikme gün sayısını hesapla (7 gün ücretsiz)
    SET @Gecikme = DATEDIFF(DAY, @AlisTarihi, @IadeTarihi) - 7;

    -- Eğer gecikme varsa ceza kaydı oluştur
    IF @Gecikme > 0
    BEGIN
        INSERT INTO CezaKayitlari (OgrenciID, OduncID, GecikmeGun, CezaMiktari, Tarih)
        VALUES (@OgrenciID, @OduncID, @Gecikme, @Gecikme * 5, GETDATE());

        PRINT 'Ceza kaydı oluşturuldu: ' + CAST(@Gecikme * 5 AS VARCHAR(10)) + ' TL';
    END
END;
```

**Çalışma Mantığı:**
1. Odunc tablosunda UPDATE işlemi olduğunda tetiklenir
2. inserted tablosundan güncellenmiş kayıt bilgileri alınır
3. İade tarihi ile alış tarihi arasındaki fark hesaplanır
4. 7 günü aşan her gün için 5 TL ceza hesaplanır
5. CezaKayitlari tablosuna otomatik kayıt eklenir

**Örnek Senaryo:**
- Öğrenci kitabı 15 günde iade etti
- Gecikme: 15 - 7 = 8 gün
- Ceza: 8 × 5 = 40 TL

### 6.5 Diğer Kullanım Alanları
- **DELETE Trigger**: Silinen kayıtları yedekleme tablosuna taşıma
- **Otomatik log kaydı**: Tüm değişiklikleri log tablosuna kaydetme
- **Stok takibi**: Satış yapıldığında stok otomatik azaltma
- **Veri doğrulama**: Hatalı veri girişini engelleme

---

## 7. ARA SINAV BİLGİLERİ

### 7.1 Sınav Tarihi ve Süresi
- **Tarih**: 22/11/2024
- **Saat**: 10:00
- **Süre**: 90 dakika

### 7.2 Soru Dağılımı
1. **Kavramsal Sorular** (20 puan)
   - İlişkisel veri tabanı tanımı
   - VTYS kavramları (tablo, PK, FK)
   - SQL alt dil grupları
   - VTYS üstünlükleri

2. **E-R Diyagramı** (35 puan)
   - Senaryo analizi
   - Varlık ve ilişki belirleme
   - Diyagram çizimi (20p)
   - Tablo dönüşümü (15p)

3. **SQL Kodlama** (15 puan)
   - CREATE DATABASE (7.5p)
   - CREATE TABLE (7.5p)

4. **SELECT Sorguları** (20 puan)
   - DISTINCT, ORDER BY
   - WHERE, LIKE
   - Subquery
   - TOP

---

## 8. ÖZET KONTROL LİSTESİ

### Temel Kavramlar
- [ ] İlişkisel veri tabanı tanımı
- [ ] VTYS, tablo, PK, FK kavramları
- [ ] SQL alt dil grupları (DDL, DML, DCL, TCL)
- [ ] VTYS üstünlükleri

### E-R Diyagramı
- [ ] İlişki türleri (1:1, 1:N, N:M)
- [ ] Varlık ve ilişki belirleme
- [ ] Tablo dönüşüm kuralları
- [ ] 3 örnek senaryo (Okul, Kargo, Kütüphane)

### SQL İşlemleri
- [ ] CREATE DATABASE söz dizimi
- [ ] CREATE TABLE (veri tipleri, IDENTITY, constraints)
- [ ] INSERT (tek ve çoklu satır)
- [ ] ALTER TABLE
- [ ] SELECT (DISTINCT, WHERE, LIKE, ORDER BY, TOP, subquery)

### JOIN İşlemleri
- [ ] RIGHT JOIN mantığı ve kullanımı
- [ ] NULL değerlerin JOIN'deki rolü

### Stored Procedure
- [ ] Tanım ve avantajları
- [ ] CREATE PROCEDURE söz dizimi
- [ ] EXEC ile çağırma
- [ ] Haftalık rapor örneği

### Trigger
- [ ] Tanım ve kullanım alanları
- [ ] AFTER UPDATE trigger
- [ ] inserted ve deleted tabloları
- [ ] Geç iade ceza hesaplama örneği

---

## 9. ÖNEMLİ NOTLAR

1. **E-R Diyagramında İlişki Belirleme:**
   - "Bir X'in birçok Y'si olabilir" → 1:N
   - "Bir X sadece bir Y'ye ait" → N:1
   - "Her ikisi de birden fazla" → N:M (ara tablo gerekir)

2. **Foreign Key Yerleşimi:**
   - 1:N ilişkide FK, N tarafına eklenir
   - N:M ilişkide ara tablo oluşturulur, her iki PK de ara tabloda FK olur

3. **LIKE Operatörü Joker Karakterler:**
   - `%`: Sıfır veya daha fazla karakter
   - `_`: Tek karakter
   - Örnek: `'M%'` → M ile başlayanlar, `'%an'` → an ile bitenler

4. **Trigger vs Stored Procedure:**
   - **Trigger**: Otomatik çalışır, INSERT/UPDATE/DELETE ile tetiklenir
   - **Stored Procedure**: Manuel çalıştırılır, EXEC komutuyla

5. **IDENTITY Kullanımı:**
   - `IDENTITY(1000,1)`: 1000'den başlar, 1'er artar
   - Primary key için ideal (otomatik ID)

---

**Hazırlayan:** Sınav Hazırlık Rehberi
**Güncelleme:** 2024-2025 Güz Dönemi
**Kapsam:** er-iliski-odev.pdf, VT_Guz_arasinav.pdf, join_sunumu.pptx, procedure.pptx, trigger.pptx
