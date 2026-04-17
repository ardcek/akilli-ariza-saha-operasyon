# Akilli Ariza ve Saha Operasyon Platformu

Bina, tesis ve kampus bakim operasyonlarinda ariza taleplerini dijital olarak yoneten, saha personeline atayan, QR dogrulama ile calismayi kanitlayan ve SLA takibi yapan entegre bir mobil operasyon platformu.

Clomosy TRObject ile gelistirilmistir.

---

## Proje Amaci ve Cozdugu Problem

Ariza ve bakim operasyonlarinda yasanan temel sorunlar:
- Talepler telefon veya kagitla iletiliyor, takip edilemiyor
- Hangi personelin ne yaptigi, nerede oldugu bilinmiyor
- Is emirleri kaybolabiliyor veya gecikmeli islenebiliyor
- SLA suresi izlenemiyor, asimlar fark edilemiyor
- Personelin gercekten sahada olup olmadigi dogrulanamiyordu

Bu platform bu sorunlari cozmek icin; talep acma, atama, durum takibi, QR dogrulama, dashboard ve raporlama modullerini tek bir uygulama altinda birlestirmektedir.

## Hedef Kullanicilar / Hedef Kurumlar

- **Ariza bildirimcileri (TalepAcan):** Bina sakinleri, kat sorumlulari, departman yetkilileri
- **Saha personeli (SahaPersoneli):** Teknisyenler, bakim ekipleri
- **Operasyon yoneticileri (Yonetici):** Tesis muduru, bakim sefi, operasyon koordinatoru
- **Hedef kurumlar:** AVM, hastane, universite kampusu, fabrika, toplu konut siteleri, kamu binalari

---

## Temel Moduller

| # | Modul | Dosya | Aciklama |
|---|---|---|---|
| 1 | Giris | `src/modules/giris/giris_module.tro` | Kimlik dogrulama ve rol bazli yonlendirme |
| 2 | Talep Acma | `src/modules/talep-acma/talep_acma_module.tro` | Yeni ariza talebi olusturma |
| 3 | Is Emri Havuzu | `src/modules/is-emri-havuzu/is_emri_havuzu_module.tro` | Acik talepleri listeleme, filtreleme, SLA takibi |
| 4 | Yonetici Atama | `src/modules/yonetici-atama/yonetici_atama_module.tro` | Talep-personel eslestirme |
| 5 | Saha Personeli | `src/modules/saha-personeli/saha_personeli_module.tro` | Atanmis isleri goruntuleme, durum guncelleme |
| 6 | QR Dogrulama | `src/modules/qr-dogrulama/qr_dogrulama_module.tro` | Sahada olma kaniti icin QR tarama simulasyonu |
| 7 | Dashboard | `src/modules/dashboard/dashboard_module.tro` | Anlik operasyon ozeti ve SLA durum paneli |
| 8 | Rapor | `src/modules/rapor/rapor_module.tro` | Tarih bazli rapor olusturma |

---

## Sistem Mimarisi

```
MainCode.tro (giris noktasi)
  ├── Tum paylasilan veri dizileri (30+ array)
  ├── Demo veri yukleme
  ├── Ortak yardimci fonksiyonlar
  └── RouterForm → TclUnit ile giris_module baslatir

giris_module
  ├── Yonetici   → is_emri_havuzu_module
  │                  ├── yonetici_atama_module
  │                  ├── talep_acma_module
  │                  ├── dashboard_module
  │                  └── rapor_module
  ├── SahaPersoneli → saha_personeli_module
  │                      └── qr_dogrulama_module
  └── TalepAcan     → talep_acma_module
```

Tum moduller TclUnit ile birbirine bagli tek bir entegre uygulama olarak calisir. Ayri ayri bagimsiz ekranlar degildir; moduller arasi veri paylasimi scope inheritance ile saglanir.

---

## Teknik Altyapi

### Clomosy / TRObject
- Dil: Clomosy TRObject
- Form yapisi: `TclStyleForm.Create(Self)` ile olusturulur, `clSetStyle(Form.LightSB)` ile stillenir
- UI bilesenleri: `TclProPanel`, `TclProLabel`, `TClProButton`, `TClProEdit`, `TclVertScrollBox`, `TclComboBox`
- Stil verme: `clComponent.SetupComponent(component, '{"JSON"}')` ile JSON tabanli konfigürasyon

### TclUnit Yapisi
- Her modul bir TclUnit olarak cagirilir:
  ```
  Unit = TclUnit.Create;
  Unit.UnitName = 'modul_adi';
  Unit.CallerForm = MevcutForm;
  MevcutForm.clHide;
  Unit.Run;
  ```
- Geri donus: `CallerForm.clShow; ModulForm.clHide;`
- Cagiran form scope'undaki tum degiskenler alt modullerde gorunurdur (scope inheritance)

### Ortak Veri Modeli
- Tum veri `MainCode.tro` icinde `TclArrayString` ve `TclArrayInteger` dizileri ile tutulur
- Moduller ortak dizilere dogrudan erisir, ayri veri dosyasi veya veritabani yoktur
- Tek canonical veri kaynagi: `MainCode.tro`

### Navigasyon Mantigi
- Giris ekrani rol kontrol eder, role gore uygun modulu TclUnit olarak baslatir
- Form gosterim olaylari `AddNewEvent(Form, tbeOnFormShow, 'handler')` ile kaydedilir
- Ekranlar arasinda geri/ileri navigasyon CallerForm referansiyla yonetilir

---

## Klasor Yapisi

```
akilli-ariza-saha-operasyon/
├── MainCode.tro                                  # Giris noktasi, ortak veri, yardimci fonksiyonlar
├── README.md
├── docs/
│   ├── 01-mimari.md
│   ├── 02-veri-modeli.md
│   ├── 03-ekranlar-ve-gorevler.md
│   ├── 04-mvp-kapsam.md
│   ├── 05-ilk-modul-secimi.md
│   ├── 06-prosedurler-giris-cikis.md
│   ├── 07-hata-kontrolleri.md
│   ├── 08-test-senaryolari.md
│   └── 09-sonraki-modul-onerisi.md
└── src/
    ├── common/
    │   ├── README.md
    │   └── veri_deposu.md
    ├── models/
    │   └── README.md
    ├── modules/
    │   ├── giris/
    │   │   └── giris_module.tro
    │   ├── is-emri-havuzu/
    │   │   └── is_emri_havuzu_module.tro
    │   ├── yonetici-atama/
    │   │   └── yonetici_atama_module.tro
    │   ├── talep-acma/
    │   │   └── talep_acma_module.tro
    │   ├── saha-personeli/
    │   │   └── saha_personeli_module.tro
    │   ├── qr-dogrulama/
    │   │   └── qr_dogrulama_module.tro
    │   ├── dashboard/
    │   │   └── dashboard_module.tro
    │   └── rapor/
    │       └── rapor_module.tro
    └── navigation/
        └── README.md
```

**Not:** Clomosy calisma ortaminda tum `.tro` dosyalari ayni proje klasorune kopyalanmalidir. Klasor yapisi reponun organizasyonu icindir; calistirma sirasinda dosyalar ayni dizinde bulunur.

---

## Demo Kullanicilari

| Kullanici Adi | Sifre | Rol | Ad Soyad | Kullanici ID |
|---|---|---|---|---|
| `yonetici1` | `1234` | Yonetici | Ayse Demir | 3001 |
| `saha1` | `1234` | SahaPersoneli | Mehmet Kaya | 2001 |
| `talep1` | `1234` | TalepAcan | Ahmet Yilmaz | 1001 |

Demo personel listesi (atama icin):

| Personel | Aktif Is (baslangic) | Personel ID |
|---|---|---|
| Mehmet Kaya | 2 | 2001 |
| Ali Celik | 1 | 2002 |
| Fatma Sahin | 0 | 2003 |

---

## Uctan Uca Demo Akisi

### 1. Giris
- Uygulamayi ac → Giris ekrani gelir
- `yonetici1` / `1234` ile giris yap → Is Emri Havuzu acilir

### 2. Is Emri Havuzu
- Acik talepler listelenir (Tamamlandi ve Iptal filtrelenmis)
- SLA asilan taleplerde kirmizi `! SLA Asildi` etiketi gorulur
- Filtre alanlarina `Yeni` veya `Kritik` yazarak filtreleme yapilabilir

### 3. Talep Secimi ve Atama
- Havuzdan `TLP-20260417-1` (Asansor arizasi) yanindaki `>` butonuna bas
- Personel Atama ekraninda personel listesini gor
- `Fatma Sahin` (0 aktif is) yaninda `Sec` butonuna bas, ardından `Personel Ata` butonuna bas
- "Basarili" mesaji gorulur, `Geri` ile havuza don

### 4. Saha Personeli Akisi
- `Geri` ile giris ekranina don, `saha1` / `1234` ile giris yap
- Atanmis isler listelenir
- `TLP-20260417-2` (Su kacagi) yanindaki `>` butonuna bas
- Detay panelinde durum bilgisi ve QR durumu gorulur

### 5. Durum Guncelleme: Atandi → Yolda
- ComboBox'tan `Yolda` sec, `Durumu Guncelle` butonuna bas
- "Basarili: Durum -> Yolda" mesaji gorulur

### 6. QR Dogrulama
- `QR Dogrula` butonuna bas → QR ekrani acilir
- QR alani: `ARIZA-TLP-20260417-2` yazilir
- `Dogrula` butonuna bas → "Basarili: QR dogrulandi" mesaji
- `Geri` ile saha ekranina don

### 7. Islemde Gecisi
- ComboBox'tan `Islemde` sec, `Durumu Guncelle` butonuna bas
- QR dogrulandigi icin gecis basarili olur

### 8. Tamamlandi
- ComboBox'tan `Tamamlandi` sec, Not alanina aciklama yaz
- `Durumu Guncelle` butonuna bas → Is listeden kaybolur, detay paneli kapanir

### 9. Dashboard
- `Geri` ile girise don, `yonetici1` / `1234` ile giris yap
- Havuzdan `Dashboard` butonuna bas
- Durum dagilimi, SLA ihlalleri ve personel is dagilimi gorulur

### 10. Rapor
- Havuza don, `Rapor` butonuna bas
- Baslangic: `16.04.2026`, Bitis: `17.04.2026` gir
- `Rapor Olustur` butonuna bas → Tablo formatinda talepler listelenir

---

## Is Kurallari

### Durum Gecis Zinciri
```
Yeni → Atandi → Yolda → Islemde → Tamamlandi
                    ↓          ↑↓
                  Beklemede ←──┘
```

Izin verilen gecisler:
| Mevcut Durum | Gecerli Sonraki Durumlar |
|---|---|
| Atandi | Yolda |
| Yolda | Islemde, Beklemede |
| Islemde | Beklemede, Tamamlandi |
| Beklemede | Islemde |

### Atama Kurallari
- Atanmamis bir is tamamlanamaz; once personele atanmalidir
- Tamamlanmis veya iptal edilmis talepler yeniden atanamaz
- Bir personele en fazla 5 aktif is atanabilir

### QR Dogrulama Kurali
- `Islemde` durumuna gecis icin QR dogrulama zorunludur
- QR formati: `ARIZA-` + talep numarasi (ornek: `ARIZA-TLP-20260417-2`)
- `ARIZA-` ile baslamayan girdiler reddedilir
- Basarili ve basarisiz denemeler loglanir

### SLA Mantigi
Talep oncelik seviyesine gore hedef kapanis suresi belirlenir:

| Oncelik | Hedef Sure |
|---|---|
| Kritik | 8 saat |
| Yuksek | 24 saat |
| Orta | 48 saat |
| Dusuk | 72 saat |

- SLA kontrolu `dd.mm.yyyy hh:nn` formatindaki tarihlerin ISO karsilastirmasiyla yapilir
- SLA asilan talepler listede kirmizi etiketle isaretlenir
- Dashboard SLA ihlal listesini gosterir

---

## Veri Modeli Ozeti

Tum veri `MainCode.tro` icindeki paralel dizilerde tutulur. Her talep ayni indeks numarasiyla tum dizilerde temsil edilir.

### Kullanici Dizileri
`ArrKulId`, `ArrKulAdi`, `ArrKulSifre`, `ArrKulRol`, `ArrKulAdSoyad`

### Talep Dizileri (13 dizi)
`ArrTalepNo`, `ArrBaslik`, `ArrAciklama`, `ArrLokasyonId`, `ArrKategoriId`, `ArrOncelik`, `ArrDurum`, `ArrAcanKullaniciId`, `ArrAtananPersonelId`, `ArrAcilisTarihi`, `ArrHedefKapanis`, `ArrKapanisTarihi`, `ArrSonNot`, `ArrQRDogrulandi`

### Personel Dizileri
`ArrPerId`, `ArrPerAd`, `ArrPerAktifIs`

### Durum Gecmisi Dizileri
`ArrDG_TalepNo`, `ArrDG_Durum`, `ArrDG_Not`, `ArrDG_IslemYapanId`, `ArrDG_Tarih`

### QR Log Dizileri
`ArrQRLog_TalepNo`, `ArrQRLog_QRIcerik`, `ArrQRLog_Durum`, `ArrQRLog_Tarih`

### Oturum Degiskenleri
`AktifKullaniciId`, `AktifKullaniciAd`, `AktifRol`, `SeciliTalepIndex`, `TalepSayisi`, `NextTalepId`, `QRLogSayisi`, `PersonelSayisi`

---

## Moduller Arasi Veri Akisi

```
MainCode.tro (veri sahibi)
       │
       ├─ giris_module: ArrKulAdi/Sifre okur → AktifKullaniciId/Rol yazar
       │
       ├─ is_emri_havuzu_module: ArrTalepNo/Durum/Oncelik okur, SeciliTalepIndex yazar
       │
       ├─ yonetici_atama_module: ArrAtananPersonelId/Durum/PerAktifIs yazar
       │
       ├─ talep_acma_module: Tum talep dizilerine yeni kayit ekler, TalepSayisi arttirir
       │
       ├─ saha_personeli_module: ArrDurum/SonNot/KapanisTarihi yazar, SeciliTalepIndex yazar
       │
       ├─ qr_dogrulama_module: ArrQRDogrulandi yazar, QR log dizilerine ekler
       │
       ├─ dashboard_module: Tum dizileri salt okur
       │
       └─ rapor_module: Tum dizileri salt okur, tarih filtresi uygular
```

Tum yazma islemleri `AddDurumLog()` araciligiyla durum gecmisine de kaydedilir.

---

## Bilinen Sinirlamalar / MVP Sinirlari

1. **Kalici depolama yok:** Veri bellekte tutulur, uygulama kapatildiginda sifirlanir
2. **Demo veri sabittir:** Uygulama her baslatildiginda 3 kullanici, 3 personel ve 5 talep yuklenir
3. **Gercek QR tarayici yok:** QR dogrulama elle metin girisi ile simule edilir
4. **Lokasyon/Kategori sayisal:** ID bazli giris yapilir, isim dokumleri yoktur
5. **Tek oturum:** Ayni anda yalnizca bir kullanici giris yapabilir
6. **Form filtreleri serbest metin:** Havuz filtreleme alanlarinda ComboBox yerine serbest girdi kullanilir
7. **Tarih girisi elle yapilir:** Rapor modulunde takvim bileseni yoktur, `dd.mm.yyyy` formatinda girilir
8. **Bildirim sistemi yok:** Push notification veya uyari mekanizmasi bulunmaz

---

## Yarisma Sunumu Icin Onerilen Demo Senaryosu

**Sure:** ~3-4 dakika

**Senaryo:**

1. **Giris** (`yonetici1` / `1234`) — Yonetici perspektifini goster
2. **Havuz** — Acik talepleri tara, SLA isaretlerini goster
3. **Atama** — `TLP-20260417-1` talebi sec, `Fatma Sahin`'e ata
4. **Dashboard** — Yonetici panelinden genel durumu goster
5. **Rapor** — `16.04.2026` - `17.04.2026` arasi rapor olustur
6. **Cikis** — Girise don
7. **Saha Girisi** (`saha1` / `1234`) — Personel perspektifine gec
8. **Is Sec** — `TLP-20260417-2` talebi sec
9. **Yolda** — Durum: Yolda olarak guncelle
10. **QR Dogrula** — `ARIZA-TLP-20260417-2` gir, dogrula
11. **Islemde** — Durum: Islemde olarak guncelle (QR zorunlulugu gecti)
12. **Tamamlandi** — Not yaz, Tamamlandi olarak guncelle, isin listeden kalktigini goster
13. **Tekrar Dashboard** — Yonetici ile giris yap, guncellenmis rakamlari goster

**Vurgulama noktalari:**
- Rol bazli erisim kontrolu
- Durum zinciri kisitlamalari (gecersiz gecis dene, hata mesajini goster)
- QR zorunlulugu (QR'siz Islemde gecisini dene, hata mesajini goster)
- SLA takibi ve gorsel uyarilar
- Canli veri guncelleme (atama sonrasi dashboard degisimi)

---

## Kurulum / Acilis Mantigi

1. Clomosy uygulamasini ac
2. Yeni bir TRObject projesi olustur
3. Repo icindeki tum `.tro` dosyalarini ayni proje klasorune kopyala:
   - `MainCode.tro` (ana dosya)
   - `giris_module.tro`
   - `is_emri_havuzu_module.tro`
   - `yonetici_atama_module.tro`
   - `talep_acma_module.tro`
   - `saha_personeli_module.tro`
   - `qr_dogrulama_module.tro`
   - `dashboard_module.tro`
   - `rapor_module.tro`
4. `MainCode.tro` dosyasini calistir
5. Giris ekrani acilir; demo kullanici bilgileriyle giris yap

**Onemli:** Clomosy'de TclUnit, unit dosyalarini proje klasorundan `UnitName` ile bulur. Dosya adlari degistirilmemelidir.

---

## Gelistirici Notlari

- Tum ortak fonksiyonlar `MainCode.tro` icindedir: `AddDurumLog`, `IsSLABreached`, `OncelikRenk`, `CalculateSLAHedefKapanis`, `GetPersonelAdById`, `InitAllArrays`, `LoadDemoData`
- Yeni modul eklemek icin: `.tro` dosyasi olustur, mevcut bir modulden `TclUnit.Create` ile cagir, `CallerForm` referansini gecir
- Modul icinden `CallerForm.clShow; ModulForm.clHide;` ile geri donus sagla
- Veri dizisi eklerken; `MainCode.tro` icinde `InitAllArrays`'e `.Create` ve `LoadDemoData`'ya test verisi ekle
- Form gosterim olaylarini kaydetmek icin: `Form.AddNewEvent(Form, tbeOnFormShow, 'HandlerAdi')`
- `IsStringNumeric()` fonksiyonu `StrToInt` oncesinde kullanilir; sayisal olmayan girdi hatasini onler
- ComboBox ogeleri `items.Add('deger')` ile eklenir, secili deger `.Text` ile okunur, sifirlama `.ItemIndex = 0` ile yapilir

---

## Dokumantasyon

Detayli analiz ve tasarim notlari `docs/` klasorundadir:

| Dosya | Icerik |
|---|---|
| `docs/01-mimari.md` | Sistem mimarisi |
| `docs/02-veri-modeli.md` | Veri yapisi detaylari |
| `docs/03-ekranlar-ve-gorevler.md` | Ekran tasarimlari ve gorevler |
| `docs/04-mvp-kapsam.md` | MVP kapsam tanimlamasi |
| `docs/05-ilk-modul-secimi.md` | Ilk modul secim karari |
| `docs/06-prosedurler-giris-cikis.md` | Giris/cikis prosedürleri |
| `docs/07-hata-kontrolleri.md` | Hata kontrol stratejisi |
| `docs/08-test-senaryolari.md` | Test senaryolari |
| `docs/09-sonraki-modul-onerisi.md` | Sonraki gelistirme onerileri |

---

## Hizli Demo Baslangici

1. Clomosy'de projeyi ac, `MainCode.tro` calistir
2. `yonetici1` / `1234` ile giris yap
3. Havuzda talep sec → personele ata
4. Girise don, `saha1` / `1234` ile giris yap
5. Is sec → Yolda → QR dogrula (`ARIZA-TLP-20260417-2`) → Islemde → Tamamlandi
6. Girise don, `yonetici1` ile Dashboard ve Rapor kontrol et

## 30 Saniyede Sistem Ozeti

Akilli Ariza Platformu; ariza taleplerini acan, saha personeline atayan, QR ile sahada olmayi dogrulayan ve SLA takibiyle operasyonu izlenebilir kilan entegre bir mobil sistemdir. Yonetici is emri havuzundan isleri atar, saha personeli durum zincirini (Atandi → Yolda → Islemde → Tamamlandi) takip eder, dashboard anlik ozet sunar, rapor modulu tarih bazli filtreleme yapar. Tum veri tek kaynaktan yonetilir, moduller TclUnit ile baglanir, QR zorunlulugu ve SLA ihlal uyarilari varsayilan is kurallarini uygular.

## Bilinen Kisitlar

- Veri bellekte tutulur, kalici degil (MVP siniri)
- QR fiziksel tarayici yerine metin girisiyle simule edilir
- Tek kullanici oturumu desteklenir
- Tarih alanlari manual girilir, takvim bileseni yoktur
- Lokasyon ve kategori ID bazlidir, isim dokumu yoktur
