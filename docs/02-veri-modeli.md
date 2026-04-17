# 02 - Veri Modeli Taslagi

## Ana Tablolar
1. Kullanicilar
- Id
- AdSoyad
- Rol (TalepAcan, SahaPersoneli, Yonetici)
- Telefon
- Eposta
- AktifMi

2. Talepler
- Id
- TalepNo
- Baslik
- Aciklama
- LokasyonId
- KategoriId
- Oncelik (Dusuk, Orta, Yuksek, Kritik)
- Durum (Yeni, Inceleniyor, Atandi, Yolda, Islemde, Beklemede, Tamamlandi, Iptal)
- AcanKullaniciId
- AtananPersonelId
- AcilisTarihi
- HedefKapanisTarihi
- KapanisTarihi
- SonNot

3. Lokasyonlar
- Id
- Ad
- Aciklama

4. Kategoriler
- Id
- Ad
- SLASaat

5. DurumGecmisi
- Id
- TalepNo
- Durum
- NotMetni
- IslemYapanKullaniciId
- IslemTarihi

6. QRLoglari
- Id
- TalepNo
- PersonelId
- IslemTipi (Baslangic, Kontrol, Kapanis)
- QRIcerik
- IslemTarihi

7. Bildirimler
- Id
- KullaniciId
- Baslik
- Icerik
- OkunduMu
- UretimTarihi

## Bu modulun eksikleri / sonraki adim
- Fiziksel DB semasi (Azure SQL/SQL Server) alan tipleri bir sonraki fazda netlestirilecek
- Index stratejisi (TalepNo, Durum, Oncelik) detaylandirilacak
