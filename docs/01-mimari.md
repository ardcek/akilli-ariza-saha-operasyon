# 01 - Mimari Yapi

## Klasor Yapisı
- docs/
  - 01-mimari.md
  - 02-veri-modeli.md
  - 03-ekranlar-ve-gorevler.md
  - 04-mvp-kapsam.md
  - 05-ilk-modul-secimi.md
  - 06-prosedurler-giris-cikis.md
  - 07-hata-kontrolleri.md
  - 08-test-senaryolari.md
  - 09-sonraki-modul-onerisi.md
- src/
  - common/
  - models/
  - navigation/
  - modules/
    - talep-acma/
      - talep_acma_module.tro

## Mimari Ilkeleri
- Moduler gelisim: Her is alani ayri modulde
- Durum akisi kontrollu: Gecersiz gecisler engellenir
- Rol bazli yetki: Talep acan, saha personeli, yonetici ayrimi
- Hata odakli akis: Kullaniciya net ve yonlendirici mesaj
- Offline onceleme: Yerel veri gecici kaydi ile devam edebilme

## Bu modulun eksikleri / sonraki adim
- Ortak navigation yardimcilari henuz ayrik dosyaya alinmadi
- Yetki denetimi merkezi bir policy yapisina alinacak
