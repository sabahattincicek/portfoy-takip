# Portföy Takip — proje kuralları

## Oturum başında (zorunlu)
- `devlog.md` dosyasının **ilk 150 satırını** oku; son durumu ve planı oradan öğren.

## Her değişiklikten sonra (zorunlu)
- `devlog.md`'ye yeni kayıt ekle: `## YYYY-MM-DD HH:MM — Başlık` + maddeler (ne, neden, test sonucu).
- **En yeni kayıt en üstte** (başlık bloğunun hemen altına) olur. Saat: `date "+%Y-%m-%d %H:%M"` ile al, tahmin etme.

## Proje
- Tek dosya uygulama: `index.html` (HTML + CSS + saf JS, internet gerektirmez, harici kütüphane yok).
- Hesap motoru `/*ENGINE-START*/ … /*ENGINE-END*/` arasında; orijinal xlsx formüllerinin birebir karşılığı. Arayüz değişikliklerinde motora dokunma; dokunulursa xlsx karşılaştırma testini yeniden çalıştır.
- Veriler tarayıcı localStorage'ında (`portfoyTakip.v1`); JSON dışa/içe aktarma biçimi geriye dönük uyumlu kalmalı.
- Yayın: GitHub `sabahattincicek/portfoy-takip`, GitHub Pages (`main` dalı, kök klasör). `Portfoy.xlsx` repoya konmaz.
- Arayüz dili Türkçe; para `1.234,56 TL`, yüzde `%5,0`, tarih `gg.aa.yyyy`.
