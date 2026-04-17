# navigation

Bu klasor ekranlar arasi gecis mantigi icindir.

## Mevcut Durum (MVP)
Navigasyon TclUnit mekanizmasi ile saglanmaktadir:
- `TclUnit.Create → .UnitName = 'modul_adi' → .CallerForm = MevcutForm → .Run`
- Geri donus: `CallerForm.clShow; MevcutForm.clHide;`

## Navigasyon Haritasi
```
MainCode → giris_module
  Yonetici    → is_emri_havuzu_module → yonetici_atama_module
                                      → talep_acma_module
                                      → dashboard_module
                                      → rapor_module
  SahaPersoneli → saha_personeli_module → qr_dogrulama_module
  TalepAcan     → talep_acma_module
```
