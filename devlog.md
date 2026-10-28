# Geliştirme Günlüğü (devlog)

> En yeni kayıt en üstte. Her kayıt: tarih saat (yerel saat, GMT+3) · ne yapıldı · neden.
> Not: 28.10.2026 14:10 öncesindeki kayıtlar oturum akışından geriye dönük yazıldı; saatleri yaklaşıktır.

---

## 2026-10-28 14:20 — Portföy planı: kaynaklar elle yönetilen listeye dönüştü
- "Kaynaklar ve linkler" artık tamamen kullanıcı listesi: önceden sabit olan 5 kaynak (TÜİK, TEFAS, KAP, TCMB, aracı kurum hesapları) ilk kayıtlar olarak duruyor ama her biri düzenlenip silinebiliyor.
- Boş "(kendi kaynağın)" satırları ve "(kendi linkini ekle)" yer tutucusu kaldırıldı; linksiz kayıtta "Link yok" yazıyor.
- "＋ Kaynak ekle" düğmesi → pencere (Konu zorunlu; açıklama, nereye girilir, link isteğe bağlı). Aynı pencere ✎ ile düzenlemede kullanılıyor. Link doğrulaması (http/https/www), Esc / dışarı tıklama ile kapanma, Tab odak döngüsü; telefonda alttan açılan panel.
- Silmede "Geri al" (aynı sıraya geri koyar).
- Veri uyumu: eski 8 satırlık kayıtlar/JSON'lar otomatik taşınır (boş yer tutucular atlanır, linkli olanlar korunur).
- Test: kaynak testi 22/22, arayüz 62/62, grafik 15/15, xlsx karşılaştırması 241/241 farksız.

## 2026-10-28 14:10 — Git reposu, GitHub ve GitHub Pages
- Yerel git reposu oluşturuldu; `portfoy.html` → `index.html` olarak yeniden adlandırıldı (Pages kök adresi için).
- `.gitignore`: orijinal `Portfoy.xlsx` repoya konmadı (public repo).
- GitHub'da `sabahattincicek/portfoy-takip` (public) reposu açıldı, `main` dalı gönderildi, GitHub Pages `main` / kök klasörden yayına alındı.
- Repo: https://github.com/sabahattincicek/portfoy-takip · Yayın: https://sabahattincicek.github.io/portfoy-takip/
- Not: Yayındaki site ile yerel dosya farklı "origin" olduğundan localStorage verisi paylaşılmaz; taşımak için JSON dışa/içe aktar.
- `devlog.md` (bu dosya) ve `CLAUDE.md` eklendi. Kural: her oturum başında `devlog.md`'nin ilk 150 satırı okunur; her yapılan iş tarih-saatle en üste yazılır.
- Sonraki plan: sekmeleri tek tek ele alıp arayüzü (özellikle tablolar ve mobil kullanım) iyileştirmek.

## 2026-10-28 ~14:00 — Grafikler sekmesi
- Yeni "5 · Grafikler" sekmesi: kütüphanesiz SVG (çevrimdışı çalışır).
- 7 karşılaştırmalı grafik: portföy değeri vs yatırılan, reel değer vs reel anapara, nominal vs reel kazanç, kümülatif getiri vs kümülatif enflasyon, aylık getiri vs TÜFE, varlık bazında aylık değer (yığılmış), hedef vs güncel ağırlık.
- Renk dili: mor `#6250d6` = portföy/sonuç, gri kesikli `#898781` = referans. Renk paleti dataviz doğrulayıcısıyla kontrol edildi.
- İpucu kutusu (fare + ←/→ tuşları), her grafikte "Veriyi tablo olarak gör", veri yoksa yönlendirici boş durum mesajı.
- Eksen hatası düzeltildi (pozitif veride eksenin −10 B'ye inmesi), eksen ondalıkları tutarlı hale getirildi.
- Test: grafik testi 15/15, arayüz testi 62/62, xlsx karşılaştırması 241/241 hücre farksız.

## 2026-10-28 ~13:45 — Kategori renkleri, sütun kaldırma
- 1. sekmeden "Örnek enstrümanlar" ve "Stratejik rolü" sütunları kaldırıldı (veri arka planda korunuyor, eski JSON'lar açılır).
- Kategori renkleri tüm satıra uygulandı (açık zemin + solda renkli şerit): plan satırları, alım kayıtları (seçilen varlığa göre), varlık özeti, aylık takipte varlık sütunları, rebalancing kartları.

## 2026-10-28 ~13:30 — Örnek veri
- `ornek-veri.json` eklendi: Nisan–Ekim 2026, 19 alım, 6 ay değerleme + TÜFE; rebalancing'de "Kâr Satışı", "Alım Yap" ve "Dengede" durumları görünür.

## 2026-10-28 ~13:15 — Arayüz yeniden tasarımı
- Excel ızgarası görünümü kaldırıldı; kartlar, gösterge kutuları, dağılım çubukları, açılır kılavuzlar ile modern arayüz.
- Hesap motoru birebir korundu (kod karşılaştırmasıyla doğrulandı).
- Mobil düzen (tek sütun), Edge headless ekran görüntüleriyle kontrol edildi.

## 2026-10-28 ~10:30 — İlk sürüm: xlsx → tek dosya HTML
- `Portfoy.xlsx` (4 sekme) openpyxl ile formülleriyle okundu, yapı özetlendi, kullanıcı onayı alındı.
- Tüm formüller JavaScript'e çevrildi (SUMIFS, VLOOKUP, INDEX, ARRAYFORMULA, EDATE; komisyon önceliği işlem → kurum → varsayılan; Ay 1 = ilk yatırım ayı; aylar bugüne kadar uzar; reel hesaplar TÜFE girilen aylar için; rebalancing = son değerleme + sonraki alımlar).
- Türkçe biçimler (1.234,56 TL · %5,0 · gg.aa.yyyy), koşullu biçimler, localStorage kaydı, JSON dışa/içe aktarma, satır ekle/sil + geri al.
- Orijinaldeki hata düzeltildi: 4. sekmedeki yeşil "Alım Yap" biçimi formül metniyle eşleşmiyordu.
- Doğrulama: aynı test verisi xlsx kopyasına yazılıp `formulas` kütüphanesiyle hesaplandı; 2 senaryoda 241/241 hücre farksız.
