# 06 - Prosedurler ve Giris/Cikis Mantigi

## Talep Acma Modul Prosedurleri

1. GenerateTalepNo
- Giris: Yok
- Cikis: String (benzersiz talep no)
- Amac: Talep no standart formatta uretmek

2. ValidateTalepForm
- Giris: Baslik, Aciklama, Lokasyon, Kategori, Oncelik
- Cikis: Boolean (gecti/gecmedi)
- Amac: Zorunlu alanlari ve temel kural kontrollerini yapmak

3. SaveTalep
- Giris: Talep alanlari
- Cikis: Boolean
- Amac: Talep kaydini veri listesine eklemek

4. AddInitialDurumKaydi
- Giris: TalepNo, KullaniciId
- Cikis: Yok
- Amac: DurumGecmisi tablosuna ilk kaydi Yeni olarak acmak

5. NavigateAfterSave
- Giris: TalepNo
- Cikis: Yok
- Amac: Sonraki ekran mantigini calistirmak (MVPde bilgi mesaji + havuz ekranina gecis noktasi)

## Bu modulun eksikleri / sonraki adim
- Prosedurler servis katmanina ayrilarak yeniden duzenlenecek
