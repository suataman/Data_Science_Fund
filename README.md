# E-Ticaret Veri Analizi ve Veri Ön İşleme Çalışması

Bu proje, bir Veri Bilimi Bootcamp eğitimi kapsamında "Veri Bilimi Temelleri" (Data Science Fundamentals) modülü dahilinde işlenen konuların ve uygulanan kodların tarafımca sıfırdan uygulanması, yapılandırılması ve geliştirilmesi amacıyla hazırlanmıştır.

Projede, bir e-ticaret platformuna ait satış ve müşteri verileri üzerinde Keşifçi Veri Analizi (EDA), Veri Temizleme (Data Cleaning) ve Veri Ön İşleme (Data Preprocessing) adımları uygulanmıştır.

Python ortamında Pandas, Matplotlib ve Seaborn kütüphaneleri kullanılarak ham veri seti analiz edilebilir, temizlenmiş ve makine öğrenmesi modellerine hazır hale getirilmiştir.

## İçindekiler

* [Veri Seti Hakkında](#veri-seti-hakkında)
* [Proje Adımları](#proje-adımları)
  * [1. Keşifçi Veri Analizi (EDA)](#1-keşifçi-veri-analizi-eda)
  * [2. Eksik Veri Temizleme (Missing Value Imputation)](#2-eksik-veri-temizleme-missing-value-imputation)
  * [3. Veri Tipi Dönüşümleri ve Özellik Türetme (Feature Engineering)](#3-veri-tipi-dönüşümleri-ve-özellik-türetme-feature-engineering)
  * [4. Metin Normalizasyonu ve Tutarsızlıkların Giderilmesi](#4-metin-normalizasyonu-ve-tutarsızlıkların-giderilmesi)
  * [5. Aykırı Değer (Outlier) Analizi ve Temizliği](#5-aykırı-değer-outlier-analizi-ve-temizliği)
  * [6. Kategorik Veri Analizi ve Görselleştirme](#6-kategorik-veri-analizi-ve-görselleştirme)
* [Çıktı Dosyaları](#çıktı-dosyaları)
* [Gereksinimler ve Kurulum](#gereksinimler-ve-kurulum)

## Veri Seti Hakkında

Projede kullanılan ham veri seti (`e_ticaret_veri_seti.csv`), 1428 satır ve 18 sütundan oluşmaktadır.

**Başlıca Sütunlar:**

* `siparis_id`, `musteri_id`: Benzersiz kimlik tanımlayıcıları
* `siparis_tarihi`: Siparişin verildiği tarih
* `sehir`, `bolge`: Müşterinin konumu
* `kategori`, `urun_adi`: Satın alınan ürün bilgileri
* `adet`, `birim_fiyat`, `toplam_tutar`: Harcama ve miktar verileri
* `odeme_turu`, `musteri_tipi`, `musteri_puani`: Müşteri ve ödeme davranışları

## Proje Adımları

### 1. Keşifçi Veri Analizi (EDA)

* `df.info()` ve `df.shape` ile veri setinin boyutu ve sütun tipleri incelendi.
* Sayısal sütunlar için `df.describe()` fonksiyonu ile istatistiksel özetler (ortalama, standart sapma, çeyreklikler, min/max) çıkarıldı.

### 2. Eksik Veri Temizleme (Missing Value Imputation)

* Veri setindeki toplam 202 eksik veri tespit edildi (özellikle `musteri_puani`, `musteri_tipi` ve `odeme_turu` alanlarında).
* Sayısal değişkenlerdeki eksikler ortalama (mean) veya medyan (median) yöntemleriyle dolduruldu.
* Kategorik değişkenlerdeki eksikler en sık tekrar eden değer (mode) ile tamamlandı.

### 3. Veri Tipi Dönüşümleri ve Özellik Türetme (Feature Engineering)

* `siparis_tarihi` sütunu `datetime64` formatına çevrildi.
* Tarih bilgisinden `siparis_yili`, `siparis_ayi`, `siparis_günü` ve `haftanin_gunu` değişkenleri türetildi.
* Kategorik alanlar bellek optimizasyonu ve analiz kolaylığı için `category` veri tipine dönüştürüldü.

### 4. Metin Normalizasyonu ve Tutarsızlıkların Giderilmesi

* Özel metin temizleme fonksiyonları (`metin_normalize_et`) yazılarak Türkçe karakter uyumsuzlukları, büyük/küçük harf hataları ve gereksiz boşluklar giderildi.
* Mantıksal hatalar tespit edildi (örneğin `musteri_puani > 5` olan hatalı kayıtlar temizlendi).

### 5. Aykırı Değer (Outlier) Analizi ve Temizliği

* IQR (Interquartile Range) yöntemi kullanılarak sayısal sütunlardaki alt ve üst sınırlar belirlendi:

  $$IQR = Q3 - Q1$$
  $$\text{Alt Sınır} = Q1 - 1.5 \times IQR$$
  $$\text{Üst Sınır} = Q3 + 1.5 \times IQR$$

* `birim_fiyat`, `toplam_tutar` ve `musteri_puani` sütunlarında tespit edilen 308 aykırı kayıt filtrelendi.

### 6. Kategorik Veri Analizi ve Görselleştirme

* `value_counts()` ile kategori, şehir ve ödeme türlerine göre frekans ve yüzde dağılımları çıkarıldı.
* Seaborn (`sns.countplot`) ve Matplotlib kütüphaneleri kullanılarak kategorik dağılım grafikleri çizdirildi.

## Çıktı Dosyaları

İşlem adımları sonucunda kaydedilen veriler:

1. `e_ticaret_veri_seti_eksik_veriler_duzenlendi.csv`: Eksik verileri tamamlanmış veri seti.
2. `e_ticaret_veri_seti_aykiri_degerler_duzenlendi.csv`: Eksik ve aykırı değerlerden arındırılmış nihai veri seti.

## Gereksinimler ve Kurulum

Projeyi yerel ortamınızda çalıştırmak için gerekli Python kütüphaneleri:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

Jupyter Notebook ortamında çalıştırmak için:

```bash
jupyter notebook e-ticaret-veri-analizi-projeleri.ipynb
```