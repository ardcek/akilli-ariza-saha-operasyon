# common

Bu klasor ortak veri modeli referansini icerir.

## Dosyalar
- `veri_deposu.tro` — Projedeki tum paylasilan dizi, oturum degiskeni ve yardimci prosedur imzalarinin kanonik referansi. Dogrudan calistirilmaz; MainCode.tro icindeki tanimlarla birebir eslenir.

## Not
Clomosy TRObject'te include/uses mekanizmasi yoktur. Tum paylasilan veriler `MainCode.tro` icerisinde tanimlanir ve TclUnit kapsam mirasi ile modullere aktarilir.
