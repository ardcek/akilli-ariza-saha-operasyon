# 08 - Test Senaryolari

## Fonksiyonel Testler
1. Zorunlu alanlar dolu iken talep kaydi acilir
- Beklenen: TalepNo olusur, ilk durum Yeni kaydedilir

2. Baslik bos iken kayit denenir
- Beklenen: Hata mesaji, kayit olmaz

3. Aciklama bos iken kayit denenir
- Beklenen: Hata mesaji, kayit olmaz

4. Rol TalepAcan degilken kayit denenir
- Beklenen: Yetki hatasi

5. Basarili kayit sonrasi yonlendirme mantigi
- Beklenen: Basari mesaji ve sonraki ekran gecis noktasi

## Kural Testleri
1. Atanmamis is Tamamlandi yapilmaya calisilir
- Beklenen: Engelleme mesaji

2. QR olmadan saha baslangici
- Beklenen: Engelleme mesaji

3. Tamamlandi notsuz gonderim
- Beklenen: Engelleme mesaji

## Negatif Testler
1. Uzun metin ve ozel karakter testleri
2. Tekrarli tiklama (double submit) testi
3. Gecersiz rol degisimi testi

## Bu modulun eksikleri / sonraki adim
- Otomasyon test plani henuz olusturulmadi
