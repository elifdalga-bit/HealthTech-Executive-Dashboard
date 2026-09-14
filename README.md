# 🚀 Sağlık Yönetimi (HBYS) Executive Dashboard

Sağlık sektöründe operasyonel verimliliği artırmak, poliklinik yüklerini analiz etmek ve üst yönetime anlık finansal/operasyonel içgörüler sunmak amacıyla geliştirilmiş **Uçtan Uca HBYS Yönetici Raporu**.

Bu çalışmada ham veritabanı mimarisinin kurgulanmasından Power BI üzerinde dinamik ve interaktif bir yönetici paneline dönüştürülmesine kadar tüm veri analitiği yaşam döngüsü uygulanmıştır.

---

## 🛠️ Proje Mimarisi & Teknoloji Yığını

* **Veritabanı & Sorgulama:** SQL Server / PostgreSQL (DDL, DML, İlişkisel Tablo Yapıları)
* **ETL & Veri Temizleme:** Power Query (Veri Tipi Dönüşümleri, Normalizasyon)
* **Veri Modelleme:** Power BI (1:N Star-Schema Mimarisi)
* **Görselleştirme & Analiz:** Power BI Desktop (DAX, Dynamic Slicers, Executive Visuals)

---

## 📐 1. Veri Tabanı Mimarisi (SQL)

Projenin omurgası 4 ana ilişkisel tablo üzerine kurgulanmıştır:

* **`Hastalar`** *(Dimension)*: Hasta demografisi ve sigorta bilgileri.
* **`Doktorlar`** *(Dimension)*: Doktor branş ve unvan detayları.
* **`Randevular`** *(Fact)*: Poliklinik bazlı randevu hareketleri ve durumları.
* **`Tedaviler_Fatura`** *(Fact)*: Gerçekleşen işlemler ve finansal tutarlar.

```sql
-- Poliklinik Bazlı Ciro ve Randevu Analiz Sorgusu
SELECT 
    r.Poliklinik,
    COUNT(DISTINCT r.Randevu_ID) AS Toplam_Randevu,
    SUM(tf.Tutar) AS Toplam_Ciro
FROM Randevular r
LEFT JOIN Tedaviler_Fatura tf ON r.Randevu_ID = tf.Randevu_ID
GROUP BY r.Poliklinik
ORDER BY Toplam_Ciro DESC;
```

---

## 🔄 2. ETL & Star-Schema Veri Modelleme

* **ETL Süreci:** Power Query kullanılarak hatalı/eksik veriler elenmiş, tarih ve tutar alanlarının veri tipleri doğrulanmıştır.
* **Modelleme:** Boyut (Dimension) tablolarından Fact tablolarına doğru 1:N tek yönlü filtre geçişine sahip performans odaklı **Star-Schema** yapısı kurulmuştur.

---

## 📊 3. Executive Dashboard Metrikleri

* **💰 Toplam Ciro (82 B TL):** Hastane genelinde elde edilen toplam finansal hacim.
* **📅 Toplam Randevu (12 Adet):** Gerçekleşen toplam hasta kabul sayısı.
* **🏥 Branş Dağılımı:** Dahiliye (4), Kardiyoloji (4) ve Göz (1) polikliniklerinin randevu yükü.
* **🛡️ Sigorta Profili:** Hasta portföyünün sigorta dağılımı (%60 SGK, %30 Özel, %10 Sigortasız).
* **🎛️ Dinamik Filtreleme:** Poliklinik seçimine göre tüm metriklerin eşzamanlı güncellenmesini sağlayan interaktif dilimleyici (Slicer).

---

## 💡 Elde Edilen İş İçgörüleri (Business Insights)

1. **Yoğunluk & Gelir:** Dahiliye ve Kardiyoloji branşları toplam randevu yükünün %88'ini sırtlayarak ana gelir sürücüsü olmuştur.
2. **Hasta Segmentasyonu:** Popülasyonun %60'ının SGK kapsamında olması, hastane operasyonlarında kamu anlaşmalarının kritik rolünü göstermektedir.
