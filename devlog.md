# Geliştirme Günlüğü (devlog)

> En yeni kayıt en üstte. Her kayıt: tarih saat (yerel saat, GMT+3) · ne yapıldı · neden.
> Not: 28.10.2026 14:10 öncesindeki kayıtlar oturum akışından geriye dönük yazıldı; saatleri yaklaşıktır.

---

## 2026-10-28 14:54 — Aracı kurumlar serbest liste oldu (8 yuva kaldırıldı)
- Kurum kartında artık yalnızca eklenen kurumlar görünüyor; boş "Kurum ekle" yuvaları yok, sayı sınırı yok. "＋ Aracı kurum ekle" listenin sonuna ekler; her satırda sil düğmesi (Geri al ile). Hiç kurum yoksa kısa yönlendirme metni.
- Kurum silinirken o kurumu kullanan kayıtlarda kurum boşaltılır ("Kurum seçilmemiş"e düşer) ki özet toplamları ve "Toplam ödenen" doğru kalsın; Geri al kayıtları da geri koyar.
- Motor: içeride 8'e kadar boş satırla tamamlanıyor, böylece 8 ve altı kurumda çıktı xlsx ile birebir aynı (M5:M12, 13 = seçilmemiş, 14 = toplam). 9. ve sonraki kurumlar 2!M15'ten itibaren satır alır, toplamlara dahil. `Engine.brokerRow(i)` eklendi.
- Veri uyumu: eski 8 yuvalı kayıtlar / JSON'lar açılırken boş yuvalar atlanır. Yeni JSON'da yalnızca dolu kurumlar var.
- Test: xlsx karşılaştırması 2 senaryoda 241/241 farksız; kurum listesi 21/21; kaynak 22/22, genel durum 16/16, grafik 15/15. `ui_test2` 47/65: önceki 17 bilinen hata + artık geçersiz olan "8 kurum satırı" kontrolü.

## 2026-10-28 14:48 — "Aracı kurum ekle" penceresi, daha geniş masaüstü düzeni
- Aracı kurumlar ve komisyon kartına "＋ Aracı kurum ekle" düğmesi → pencere: Kurum adı (zorunlu), komisyon oranı % (isteğe bağlı, 0–10; boşsa varsayılan oran).
- Yeni kurum ilk boş kurum yuvasına yazılır (yapı 8 yuvalı kalır; motor ve JSON biçimi değişmedi). Aynı ad (büyük/küçük harf duyarsız) reddedilir; 8 yuva doluysa düğme pasif ve nedenini söylüyor, bir yuva boşalınca yeniden etkin.
- Kaynak penceresindeki ortak davranış (Esc / dışarı tıklama, Tab odak döngüsü) `openDialog` yardımcı işlevine alındı; kaynak ve kurum pencereleri bunu kullanıyor.
- Masaüstü içerik genişliği 1320 → 1440 px (yan boşluklar biraz daha kullanılıyor).
- Test: kurum penceresi 15/15; kaynak 22/22, genel durum 16/16, grafik 15/15. `ui_test2` 48/65, ama önceki commit'te de aynı 17 hata var (bu değişiklikten değil; 3. ve 4. sekme hücreleri ve kayıt kontrolleri, ayrıca bakılmalı).

## 2026-10-28 14:40 — İşlem kaydı: Alım kayıtları kartı en alta
- İşlem kaydı sekmesinde kart sırası: gösterge kutuları → Varlık / Aracı kurum özetleri → Aracı kurumlar ve komisyon → Sütunlar ve ipuçları → Alım kayıtları (önceden alım kayıtları kutuların hemen altındaydı).
- Yalnızca sıralama değişti; motor ve veri yapısı aynı.
- Test: Edge headless ekran görüntüsüyle sıra doğrulandı.

## 2026-10-28 14:31 — "Portföy" sekmesi ve Genel durum
- 1. sekmenin adı "Portföy planı" → "Portföy"; diğer sekmelerdeki göndermeler de güncellendi.
- Sekmenin en üstüne sade "Genel durum" bölümü (4 kutu, ek veri girişi yok):
  - Cebinden çıkan = toplam ödenen, komisyon dahil (3. tablo B7) · alım sayısı ve toplam komisyon.
  - Güncel değer = son değerleme (B10) · hangi aya ait olduğu (B6).
  - Kâr / zarar = B11, yüzde B12 (son değerlemeye kadar yatırılana göre).
  - Reel kâr / zarar = B16, yüzde = B16 / B14 (yeni türetilmiş gösterim; motor değişmedi).
- Değerleme / TÜFE yoksa 0 TL yerine "—" ve ne girileceğini söyleyen kısa yönlendirme; hiç alım yoksa tek satırlık başlangıç mesajı.
- Son değerlemeden sonra yapılan alım varsa (B7 − B9) tek satırlık bilgi notu: "henüz değerlemeye dahil değil".
- Test: genel durum 16/16, arayüz 65/65, kaynak 22/22, grafik 15/15, xlsx karşılaştırması 241/241 farksız.

## 2026-10-28 14:27 — Ayarlar ilgili sekmelere taşındı
- Portföy planı'ndaki "Ayarlar" kartı kaldırıldı; sekmede artık yalnızca hedef dağılım (tam genişlik) ve kaynaklar var.
- Varsayılan komisyon oranı + 8 aracı kurum → İşlem kaydı sekmesinde yeni "🏦 Aracı kurumlar ve komisyon" kartı (özet tablolarının altında; kurumlar masaüstünde 2 sütun, telefonda tek sütun). Kartta öncelik sırası yazıyor: işleme özel oran → kurum oranı → varsayılan oran.
- Rebalancing sapma eşiği → Rebalancing sekmesinin üstündeki kutu artık düzenlenebilir giriş (değişince aksiyonlar anında güncellenir).
- Rehber/açıklama metinleri yeni yerlere göre güncellendi; varsayılan "Aracı kurum hesapların" kaynağının metni "İşlem kaydı → Aracı kurumlar" oldu.
- Veri yapısı ve hesap motoru değişmedi (hücre anahtarları yine 1. tablonun B4, B5, F4:H11'i); kayıtlı veriler ve JSON yedekleri aynen çalışır.
- Test: arayüz 65/65, kaynak 22/22, grafik 15/15, xlsx karşılaştırması 241/241 farksız.

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
