# Veri Modeli Referansi

Bu dosya `MainCode.tro` icerisinde tanimlanan paylasilan veri yapisinin **salt okunur referansidir**.
Kod buraya yazilmaz. Tek kanonik kaynak: **MainCode.tro**.

## Oturum Degiskenleri

| Degisken | Tip | Aciklama |
|---|---|---|
| AktifKullaniciId | Integer | Giris yapan kullanicinin ID'si |
| AktifKullaniciAd | String | Giris yapan kullanicinin ad-soyad |
| AktifRol | String | TalepAcan, SahaPersoneli, Yonetici |

## Navigasyon

| Degisken | Tip | Aciklama |
|---|---|---|
| SeciliTalepIndex | Integer | Havuz/Saha'dan alt modüle aktarilan talep indeksi |

## Kullanicilar (ArrKul*)

| Dizi | Tip |
|---|---|
| ArrKulId | TclArrayInteger |
| ArrKulAdi | TclArrayString |
| ArrKulSifre | TclArrayString |
| ArrKulRol | TclArrayString |
| ArrKulAdSoyad | TclArrayString |

## Talepler (Arr*)

| Dizi | Tip | Aciklama |
|---|---|---|
| ArrTalepNo | TclArrayString | TLP-yyyymmddhhnnss-N |
| ArrBaslik | TclArrayString | |
| ArrAciklama | TclArrayString | |
| ArrLokasyonId | TclArrayInteger | |
| ArrKategoriId | TclArrayInteger | |
| ArrOncelik | TclArrayString | Dusuk, Orta, Yuksek, Kritik |
| ArrDurum | TclArrayString | Yeni, Atandi, Yolda, Islemde, Beklemede, Tamamlandi, Iptal |
| ArrAcanKullaniciId | TclArrayInteger | |
| ArrAtananPersonelId | TclArrayInteger | 0 = atanmamis |
| ArrAcilisTarihi | TclArrayString | dd.mm.yyyy hh:nn |
| ArrHedefKapanis | TclArrayString | dd.mm.yyyy hh:nn (SLA) |
| ArrKapanisTarihi | TclArrayString | bos veya dd.mm.yyyy hh:nn |
| ArrSonNot | TclArrayString | |
| ArrQRDogrulandi | TclArrayInteger | 0 = hayir, 1 = evet |
| TalepSayisi | Integer | Toplam talep adedi |
| NextTalepId | Integer | Sonraki talep sekans numarasi |

## Personel (ArrPer*)

| Dizi | Tip | Aciklama |
|---|---|---|
| ArrPerId | TclArrayInteger | |
| ArrPerAd | TclArrayString | |
| ArrPerAktifIs | TclArrayInteger | Maks 5 |
| PersonelSayisi | Integer | |

## Durum Gecmisi (ArrDG_*)

| Dizi | Tip |
|---|---|
| ArrDG_TalepNo | TclArrayString |
| ArrDG_Durum | TclArrayString |
| ArrDG_Not | TclArrayString |
| ArrDG_IslemYapanId | TclArrayInteger |
| ArrDG_Tarih | TclArrayString |

## QR Log (ArrQRLog_*)

| Dizi | Tip |
|---|---|
| ArrQRLog_TalepNo | TclArrayString |
| ArrQRLog_QRIcerik | TclArrayString |
| ArrQRLog_Durum | TclArrayString |
| ArrQRLog_Tarih | TclArrayString |
| QRLogSayisi | Integer |

## Yardimci Fonksiyonlar (MainCode.tro)

| Fonksiyon | Imza | Islem |
|---|---|---|
| AddDurumLog | (TalepNo, Durum, Not: String; YapanId: Integer) | DG dizilerine log ekler |
| IsSLABreached | (HedefStr: String): Boolean | ISO format ile SLA asim kontrolu |
| OncelikRenk | (Oncelik: String): String | Oncelik renk kodu doner |
| CalculateSLAHedefKapanis | (OncelikText: String): String | Oncelik bazli hedef kapanis tarihi |
| GetPersonelAdById | (PerId: Integer): String | Personel ID'den ad lookup |
