# 01 - Mimari Yapi

## Klasor Yapisi

```
MainCode.tro                 <- Ana giris noktasi, tum paylasilan veri ve yardimcilar
src/
  common/
    veri_deposu.tro          <- Kanonik veri modeli referansi (dokumantasyon)
    README.md
  models/
    README.md
  navigation/
    README.md
  modules/
    giris/
      giris_module.tro
    talep-acma/
      talep_acma_module.tro
    is-emri-havuzu/
      is_emri_havuzu_module.tro
    yonetici-atama/
      yonetici_atama_module.tro
    saha-personeli/
      saha_personeli_module.tro
    qr-dogrulama/
      qr_dogrulama_module.tro
    dashboard/
      dashboard_module.tro
    rapor/
      rapor_module.tro
docs/
  01-mimari.md ... 09-sonraki-modul-onerisi.md
```

## Calisma Modeli
- `MainCode.tro` tum paylasilan dizileri tanimlar, demo veriyi yukler, `giris_module` u TclUnit olarak baslatir.
- Her modul TclUnit olarak calisir ve parent scope uzerinden MainCode degiskenlerine erisir.
- Navigasyon: `TclUnit.Create → UnitName → CallerForm → Run` / Geri donus: `CallerForm.clShow + clHide`

## Mimari Ilkeleri
- Moduler gelisim: Her is alani ayri modulde
- Tek veri kaynagi: MainCode.tro icerisinde TclArrayString/TclArrayInteger dizileri
- TclUnit kapsam mirasi: Alt unitler ust scope degiskenlerine dogrudan erisir
- Durum akisi kontrollu: saha_personeli_module icerisinde DurumGecisiGecerliMi fonksiyonu
- Rol bazli yetki: Giris modulu rol bazli yonlendirme yapar
- SLA kontrolu: IsSLABreached fonksiyonu ISO format (yyyymmddhhnn) ile karsilastirir

## Sonraki Adim
- Lokasyon ve kategori dizileri henuz sabit, dinamik hale getirilecek
- Yetki denetimi merkezi policy yapisina alinacak
