# Akilli Ariza ve Saha Operasyon Platformu

Bu repo, Clomosy TRObject ile gelistirilen yarisma projesinin MVP uygulama iskeletini ve dokumantasyonunu icerir.

## Proje Amaci
- Manuel ariza ve saha operasyon takibini dijitallestirmek
- Is emri kaybini ve gecikmeleri azaltmak
- Yoneticinin is akisini tek ekrandan takip etmesini saglamak
- Saha personelinin dogru talebe, dogru sirayla mudahale etmesini kolaylastirmak
- QR dogrulama ve durum gecmisi ile operasyon izlenebilirligini artirmak

## MVP Kapsami
Bu surumde asagidaki ana moduller olusturuldu:

1. Giris
2. Talep Acma
3. Is Emri Havuzu
4. Yonetici Atama
5. Saha Personeli Is Listesi
6. QR Dogrulama
7. Dashboard
8. Rapor

## Modul Ozeti

### 1. Giris
Dosya: src/modules/giris/giris_module.tro

- Demo kullanici girisi yapar
- Rol bazli yonlendirme mantigi barindirir
- Demo roller: TalepAcan, SahaPersoneli, Yonetici

### 2. Talep Acma
Dosya: src/modules/talep-acma/talep_acma_module.tro

- Yeni ariza kaydi olusturur
- Talep numarasi uretir
- Oncelik, lokasyon, aciklama ve SLA baslangic bilgilerini toplar
- Ilk durum kaydini olusturur

### 3. Is Emri Havuzu
Dosya: src/modules/is-emri-havuzu/is_emri_havuzu_module.tro

- Acik talepleri listeler
- Durum ve oncelige gore filtreleme yapar
- SLA asimi durumlarini isaretler
- Yonetici secimi icin havuz ekranini saglar

### 4. Yonetici Atama
Dosya: src/modules/yonetici-atama/yonetici_atama_module.tro

- Secilen is emrini saha personeline atar
- Atama kurallarini kontrol eder
- Durum gecmisi kaydi olusturur

### 5. Saha Personeli Is Listesi
Dosya: src/modules/saha-personeli/saha_personeli_module.tro

- Aktif personele atanmis isleri listeler
- Durum akisini yonetir: Atandi -> Yolda -> Islemde -> Beklemede -> Tamamlandi
- Tamamlandi icin not zorunlulugu uygular
- QR olmadan isleme baslama kisitini gosterir

### 6. QR Dogrulama
Dosya: src/modules/qr-dogrulama/qr_dogrulama_module.tro

- QR okutma senaryosunu MVP seviyesinde simule eder
- Talep numarasi ile QR icerigini dogrular
- Basarili/basarisiz okutmalari loglar

### 7. Dashboard
Dosya: src/modules/dashboard/dashboard_module.tro

- Acik, beklemede, tamamlandi ve iptal sayilarini ozetler
- Yonetim icin hizli durum gorunumu sunar
- SLA ve operasyon ozetini ust seviyede gosterir

### 8. Rapor
Dosya: src/modules/rapor/rapor_module.tro

- Tarih araligi bazli rapor olusturur
- Talep satirlarini ozet olarak listeler
- Rapor metnini panoya kopyalar

## Demo Kullanici Bilgileri
Giris modulu icindeki demo hesaplar:

- Kullanici: talep1  Sifre: 1234  Rol: TalepAcan
- Kullanici: saha1   Sifre: 1234  Rol: SahaPersoneli
- Kullanici: yonetici1  Sifre: 1234  Rol: Yonetici

## Dizin Yapisi

- docs/: Mimari, veri modeli, ekranlar, MVP kapsami ve test senaryolari
- src/common/: Ortak yapilar icin yer ayirici klasor
- src/models/: Veri modeli notlari
- src/navigation/: Ekran gecis tasarimi notlari
- src/modules/: Tum TRObject modulleri

## Dokumantasyon
Detayli analiz ve tasarim notlari docs klasorundadir:

- docs/01-mimari.md
- docs/02-veri-modeli.md
- docs/03-ekranlar-ve-gorevler.md
- docs/04-mvp-kapsam.md
- docs/05-ilk-modul-secimi.md
- docs/06-prosedurler-giris-cikis.md
- docs/07-hata-kontrolleri.md
- docs/08-test-senaryolari.md
- docs/09-sonraki-modul-onerisi.md

## Mimari
- Tum moduller TclUnit ile birbirine bagli tek bir entegre uygulama olarak calisir
- MainCode.tro giris noktasidir ve tum ortak veriyi (array ve degiskenler) barindirir
- Her modul TclUnit.CallerForm ile scope inheritance kullanarak ortak veriye erisir
- Ekran gecisleri: CallerForm.clHide → Unit.Run → CallerForm.clShow

## Teknik Yapi
- Dil: Clomosy TRObject
- Form yapisi: TclStyleForm
- UI bilesenleri: TclProPanel, TclProLabel, TClProButton, TClProEdit, TclVertScrollBox, TclComboBox
- Veri tutma yaklasimi: TclArrayString, TclArrayInteger (MainCode.tro tek kaynak)
- Stil verme: clComponent.SetupComponent(JSON)
- Form olaylari: AddNewEvent(Form, tbeOnFormShow, 'handler')
- Modul navigasyonu: TclUnit ile CallerForm scope miras alma

## Tamamlanan Ozellikler
- Rol bazli giris ve yonlendirme (TalepAcan, SahaPersoneli, Yonetici)
- Talep acma: ComboBox ile oncelik secimi, validasyon, SLA hedef kapanis hesaplama
- Is emri havuzu: durum/oncelik filtresi, SLA asimi isaretleme
- Yonetici atama: personel is yukleme kontrolu (max 5), durum gecmisi
- Saha personeli: durum zinciri kontrolu, ComboBox ile durum secimi, otomatik liste guncelleme
- QR dogrulama: ARIZA- prefix kontrolu, dogrulama loglama, scope uzerinden etiket guncelleme
- Dashboard: durum sayilari, SLA ihlalleri, personel is dagilimi
- Rapor: tarih araligi (dd.mm.yyyy) filtresi, ISO karsilastirma
- StrToInt guvenlik: IsStringNumeric fonksiyonu ile sayi validasyonu
- Form otomatik yenileme: tbeOnFormShow ile ekran gosteriminde veri guncelleme
