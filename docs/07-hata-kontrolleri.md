# 07 - Hata Kontrolleri

## Talep Acma icin zorunlu kontroller
1. Baslik bos olamaz
2. Aciklama bos olamaz
3. Lokasyon bos olamaz
4. Kategori bos olamaz
5. Oncelik bos olamaz
6. Talep acan rol disinda kayit engellenir

## Is kurallarina dayali kontroller
1. Kritik isler havuzda ustte listelenir
2. Atanmamis is Tamamlandi durumuna gecemez
3. QR dogrulama olmadan Islemde baslangici kesinlesmez
4. Tamamlandi durumunda kapanis notu zorunlu
5. Beklemede durumunda aciklama zorunlu
6. SLA asimi yonetici ekraninda gorunur

## Kullaniciya donen mesajlar
- Hata: [alan] bos birakilamaz
- Hata: Bu islemi yapmak icin yetkiniz yok
- Basarili: Talep kaydedildi

## Bu modulun eksikleri / sonraki adim
- Merkezi hata kodu tablosu henuz tanimlanmadi
- Yerel dil dosyasi ile mesaj yonetimi sonraki fazda eklenecek
