# 👥 SAP ABAP – Personel Takip Sistemi

## 📋 Proje Özeti

Bu proje, **SAP ABAP** ortamında geliştirilmiş bir **Personel Takip Sistemi**dir. Program iki ana işleve sahiptir:
- **Yeni Personel Kaydı**: Editable ALV Grid üzerinden personel bilgilerini girerek veritabanına kaydetme
- **Personel Listeleme**: Çoklu filtreleme kriterleriyle personel sorgulama, maaş hesaplama ve yıllık izin belirleme

---

## 🎯 Projenin Amacı

Bir şirketteki personel yönetiminin temel ihtiyaçlarına yanıt vermek:

> *"Personel bilgilerini kaydet, departman ve unvana göre filtrele, tecrübeye dayalı maaş ve izin haklarını otomatik hesapla."*

---

## ⚙️ Teknik Detaylar

| Özellik | Detay |
|---|---|
| **Platform** | SAP ERP (ABAP) |
| **Dil** | ABAP (Object-Oriented) |
| **Mimari** | OOP – `lcl_class_0100` ve `lcl_class_0200` sınıfları |
| **Ekranlar** | 2 Dynpro Ekranı (0100: Kayıt, 0200: Listeleme) |
| **Çıktı** | Editable + Read-Only ALV Grid |
| **Veritabanı** | 3 Z Tablosu (INNER JOIN ile ilişkili) |

### 📊 Veritabanı Tabloları

#### `Z180_PERSBLG_T` – Personel Bilgi Tablosu

| Alan | Tip | Açıklama |
|---|---|---|
| `PERSONEL_ID` | CHAR(10) | Personel numarası |
| `PERSONEL_AD` | CHAR(20) | Ad |
| `PERSONEL_SOYAD` | CHAR(20) | Soyad |
| `PERSONEL_DEPARTMAN_ID` | CHAR(10) | Departman kodu |
| `PERSONEL_TITLE` | CHAR(10) | Unvan kodu |
| `TOPLAM_TECRUBE` | INT4 | Toplam tecrübe (yıl) |

#### `Z180_DEPT_T` – Departman Tablosu

| Alan | Tip | Açıklama |
|---|---|---|
| `DEPARTMAN_ID` | CHAR(10) | Departman kodu |
| `DEPARTMAN_AD` | CHAR(30) | Departman adı |

#### `Z180_TITLEBLG_T` – Unvan Tablosu

| Alan | Tip | Açıklama |
|---|---|---|
| `TITLE_ID` | CHAR(10) | Unvan kodu |
| `TITLE_AD` | CHAR(30) | Unvan adı |
| `MAAS_KATSAYISI` | DEC(3) | Maaş katsayısı |

---

## 🧠 İş Mantığı

### Ekran 1 – Yeni Personel Kaydı (Screen 0100)
1. Kullanıcı, editable ALV Grid üzerinden personel bilgilerini girer.
2. **Kaydet** butonuna basıldığında her satır için:
   - Tüm alanların dolu olup olmadığı kontrol edilir.
   - Girilen `PERSONEL_TITLE` ve `PERSONEL_DEPARTMAN_ID` değerleri ilgili tablolarda doğrulanır.
   - Doğrulama başarılıysa veri `Z180_PERSBLG_T` tablosuna eklenir ve satır **yeşile** (C510) döner.
   - Başarılı eklenen satırlar **düzenlemeye kapatılır** (disable editing).

### Ekran 2 – Personel Listeleme (Screen 0200)
1. Selection Screen üzerinden 4 farklı filtre ile sorgulama yapılır:
   - `GSO_PA` → Personel Adı
   - `GSO_PSA` → Personel Soyadı
   - `GSO_PDA` → Departman Adı
   - `GSO_PTA` → Unvan Adı
2. 3 tablo **INNER JOIN** ile birleştirilir.
3. Duplicate kayıtlar `personel_id` bazında silinir.
4. **Maaş Hesaplama**: `Toplam Tecrübe × Maaş Katsayısı × 2000`
5. **Yıllık İzin Hesaplama**:

| Tecrübe (yıl) | Yıllık İzin |
|---|---|
| < 1 | YOK |
| 1 – 2 | 1 Hafta |
| 3 – 4 | 2 Hafta |
| 5 – 9 | 3 Hafta |
| 10+ | 1 Ay |

---

## 🖥️ Ekran Görüntüleri

### Selection Screen – İşlem Seçimi (Yeni Kayıt)

![Selection Screen - Yeni Kayıt](Resimler/Ekran%20görüntüsü%202026-02-26%20235058.png)

### Selection Screen – Listeleme ve Filtreler

![Selection Screen - Filtreler](Resimler/Ekran%20görüntüsü%202026-02-26%20235350.png)

### Editable ALV – Yeni Personel Kaydı (Yeşil: Başarılı)

![Editable ALV](Resimler/Ekran%20görüntüsü%202026-02-26%20235156.png)

### Listeleme ALV – Filtrelenmiş Sonuç (Maaş + Yıllık İzin)

![Listeleme ALV](Resimler/Ekran%20görüntüsü%202026-02-26%20235528.png)

### Personel Bilgi Tablosu – `Z180_PERSBLG_T`

Tablo yapısı:

![Z180_PERSBLG_T Yapısı](Resimler/Ekran%20görüntüsü%202026-02-26%20234811.png)

Tablo verileri:

![Z180_PERSBLG_T Verileri](Resimler/Ekran%20görüntüsü%202026-02-26%20234835.png)

### Departman Tablosu – `Z180_DEPT_T`

Tablo yapısı:

![Z180_DEPT_T Yapısı](Resimler/Ekran%20görüntüsü%202026-02-26%20234904.png)

Tablo verileri:

![Z180_DEPT_T Verileri](Resimler/Ekran%20görüntüsü%202026-02-26%20234911.png)

### Unvan Tablosu – `Z180_TITLEBLG_T`

Tablo yapısı:

![Z180_TITLEBLG_T Yapısı](Resimler/Ekran%20görüntüsü%202026-02-26%20234922.png)

Tablo verileri:

![Z180_TITLEBLG_T Verileri](Resimler/Ekran%20görüntüsü%202026-02-26%20234933.png)

---

## 🔑 Öne Çıkan Teknik Özellikler

- **✏️ Editable ALV Grid**: Kullanıcı doğrudan ALV üzerinden veri girer, `register_edit_event` ile anlık değişiklikler yakalanır.
- **🎨 Satır Renklendirme**: Başarılı kayıtlar yeşile döner (`LINE_COLOR = C510`).
- **🔒 Dinamik Disable Editing**: `lvc_s_styl` ile başarılı satırlar düzenlemeye kapatılır.
- **🔗 INNER JOIN Sorguları**: 3 tablo ilişkilendirilerek tek sorguda zengin veri çekilir.
- **🔍 Çoklu Filtreleme**: `SELECT-OPTIONS` ile ad, soyad, departman ve unvan bazında filtreleme.
- **💰 Otomatik Maaş & İzin Hesaplama**: Tecrübe ve katsayıya dayalı dinamik hesaplama.
- **📻 Radio Button ile Ekran Yönlendirme**: `USER-COMMAND` ile Selection Screen dinamik kontrol.

---

## 🛠️ Kullanılan Teknolojiler

`SAP ERP` · `ABAP OOP` · `ALV Grid (Editable)` · `ABAP Dictionary` · `Dynpro` · `Selection Screen` · `INNER JOIN` · `SELECT-OPTIONS`

---

## 📂 Proje Yapısı

```
Z_180_PERSTAKIP
├── Z_180_PERSTAKIP_top   → Global veri tanımlamaları, TYPES, Selection Screen
├── Z_180_PERSTAKIP_cls   → Sınıf tanımı ve implementasyonu (0100 + 0200)
└── Z_180_PERSTAKIP_mdl   → Modül havuzu (PBO/PAI her iki ekran için)
```

---

## 👨‍💻 Geliştirici

Bu proje, SAP ABAP eğitimi kapsamında geliştirilmiştir.

---

> **#SAP #ABAP #ALV #PersonelYönetimi #HRModule #EditableALV #OOP #SAPDevelopment #ERPDevelopment #INNERJOIN**
