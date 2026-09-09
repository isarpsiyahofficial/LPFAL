# LP FAL — Functional UI Gates

Bu dosya, `design_refs/ui/` altındaki üretilmiş ekran mockup'larının uygulamada nasıl hayata geçirileceğini bağlayıcı olarak tanımlar. Mockup dosyaları **yalnız görsel referanstır**; ekranın tamamı runtime'da resim olarak gösterilemez. Metinler, butonlar, kartlar, navigasyon, progress, chat balonları ve etkileşimler Flutter widget'larıyla gerçek işlevli olarak uygulanacaktır.

## 1. V1 kapsamı korunacak
- V1 gerçek modüller: Kahve Falı, Tarot Falı, El Falı, aktif fala bağlı AI sohbeti, geçmiş fallar, Premium, profil/ayarlar, reklamlar ve cihaz içi Qwen.
- Mockup'larda görünen Rüya Tabiri, Günlük Yorum, Aşk Uyumu, Kariyer modülü vb. V1 kapsamına otomatik olarak girmez.
- Premium ürün modeli ve kullanıcıya görünen adlandırmada `PRODUCT_MODEL_V1.md` ana kaynaktır.
- Çatışma halinde `PRODUCT_MODEL_V1.md`, `SPECIFICATION.md`, `TODO.md`, `V1_RELEASE_GATES.md`, `MONETIZATION_V1.md` ve bu dosya birlikte uygulanır; Premium isimlendirmesinde `PRODUCT_MODEL_V1.md` önceliklidir.

## 2. Gerçek AI sohbeti — statik görsel yasak
- Sohbet ekranı gerçek, kaydırılabilir mesaj listesi içerecek.
- Kullanıcı metin alanına yazacak ve gönder butonuyla mesaj gönderecek.
- Kullanıcı ve Qwen mesajları ayrı balonlarda, doğru sırayla gösterilecek.
- Qwen yanıt üretirken loading/generating durumu gösterilecek; UI ana thread'i bloklanmayacak.
- Üretim iptal/retry akışı desteklenecek.
- Sohbet yalnız aktif fal bağlamında çalışacak; genel amaçlı asistan olmayacak.
- Kahve bağlamı: merged structured cup analysis + final result + sınırlandırılmış sohbet geçmişi.
- Tarot bağlamı: seçilen kart metadata'ları + pozisyonlar + yönler + final yorum + sınırlandırılmış geçmiş.
- El Falı bağlamı: structured palm analysis + final result + sınırlandırılmış geçmiş.
- Sohbet ve prompt cihaz içi Qwen ile işlenecek; AI inference için sunucuya gönderilmeyecek.
- Free kullanıcı: ilk takip sorusu ücretsiz; ardından her 3 kullanıcı mesajı paketi için 2 Rewarded Ad. Premium: reklam kapısı yok.
- Chat geçmişi yerel saklanacak ve yeni fal başladığında yeni conversation context açılacak.
- Sağlık, hukuk, finans, ölüm, hamilelik vb. alanlarda kesinlik veya yönlendirici tavsiye üretilmeyecek.

## 3. Android kamera ve galeri — gerçek entegrasyon
### Fotoğraf Çek
- Kahve Falı ve El Falı ekranındaki `Fotoğraf Çek` butonu gerçek Android kamera akışını açacak.
- Kamera yalnız kullanıcı butona bastığında çağrılacak.
- Gerekiyorsa `android.permission.CAMERA` AndroidManifest'e eklenecek ve runtime permission istenecek.
- Permission reddedilirse uygulama çökmeden açıklama + tekrar dene sunacak.
- `Don't ask again` / permanently denied halinde kullanıcıya sistem Ayarları'nı açma seçeneği sunulacak.
- İzin verilmeden kamera başlatılmayacak.

### Galeriden Seç
- `Galeriden Seç` gerçek Android System Photo Picker kullanacak.
- Android 13+ için geniş medya/storage izni istemeden Photo Picker tercih edilecek.
- Desteklenen eski Android sürümlerinde sistem picker/backport kullanılacak.
- `READ_MEDIA_IMAGES` veya geniş depolama izni yalnız teknik olarak kaçınılmazsa ve release incelemesiyle eklenecek; varsayılan çözüm değildir.
- Kullanıcı iptal ederse ekran güvenli şekilde aynı durumda kalacak.

### Fotoğraf güvenliği
- EXIF/orientation normalize edilecek.
- Blur, aşırı karanlık/aydınlık, yanlış içerik, yetersiz çözünürlük ve kadraj doğrulanacak.
- Ham fotoğraflar app-private geçici alanda tutulacak.
- Fal üretiminden sonra ham fotoğraflar varsayılan olarak silinecek.
- Fotoğraf yolu, prompt, structured analysis veya chat production loglarına yazılmayacak.

## 4. Kahve Falı ekranı
- Mockup'taki kamera çerçevesi gerçek fotoğraf preview alanıdır.
- Kullanıcı 2–3 fotoğraf ekleyebilir; 3 fotoğraf önerilir.
- Fotoğraf kartları ekleme/silme/yeniden çekme işlevine sahip olacak.
- `Analiz Et` ancak minimum geçerli fotoğraf şartı sağlanınca aktif olacak.
- AI analiz ekranı gerçek inference durumunu gösterecek; sahte yüzde ilerleme kullanılmayacak. Yüzde gösterilecekse gerçek pipeline state'inden üretilecek.

## 5. El Falı ekranı
- Kullanıcı 1–2 avuç içi fotoğrafı ekleyebilir.
- El/avuç görünürlüğü ve çekim kalitesi doğrulanır.
- Parmak izi template'i, kimlik doğrulama veya biyometrik kişi eşleştirme yapılmaz.
- Sağlık, yaşam süresi, hamilelik, hassas özellik veya kesin kişilik çıkarımı yapılmaz.

## 6. Tarot ekranı
- Tarot kartları statik screenshot değildir; gerçek tıklanabilir Flutter widget'larıdır.
- Backend deck 78 benzersiz karttan oluşur.
- Aynı spread içinde aynı kart tekrar seçilemez.
- Kart seçimi güvenli/unbiased RNG ile yapılır ve kullanıcı dokunuşu gerçek selection state'i değiştirir.
- 1, 3 ve 5 kart açılımları desteklenir.
- Kart pozisyonu, upright/reversed durumu ve kullanıcının sorusu Qwen'e structured metadata olarak verilir.
- Qwen kart seçmez veya yeni kart uydurmaz; yalnız verilen kartları yorumlar.

## 7. Navigasyon, Premium ve butonlar
- Dashboard Kahve/Tarot/El kartları gerçek route/navigation butonlarıdır.
- Bottom navigation gerçek sayfa/state navigasyonu yapar.
- Premium butonu Google Play tek seferlik satın alma ekranına/akışına bağlanır.
- Kullanıcıya görünen plan adı yalnız `LP FAL Premium` / `Premium` olacaktır.
- `PRO`, `Pro`, `Ömür Boyu Premium` ve `Lifetime Premium` kullanıcı metni olarak kullanılmaz.
- Satın alma açıklaması: `Tek seferlik satın alım · Abonelik değildir.`
- Satın alma CTA'sı: `Premium'a Geç` veya `Premium'u Aç`.
- Restore CTA'sı: `Satın Alımı Geri Yükle`.
- Geri, gönder, kamera, galeri, kart seç, analiz, retry ve restore kontrollerinin tamamı gerçek callback/state'e bağlıdır.
- Görsel üstüne basılmış sahte UI metni/button kabul edilmez.

## 8. Responsive ve erişilebilirlik
- Telefon 360dp'den büyük telefon/tablet/BlueStacks'a kadar overflow olmamalı.
- Safe area, notch, gesture navigation desteklenmeli.
- Kritik touch target >=48dp.
- TalkBack için anlamlı semantic label bulunmalı.
- Font scaling kritik ekranları bozmamalı.

## 9. Kabul testi — bloklayıcı
Aşağıdakiler manuel/otomatik test edilmeden ilgili UI fazı tamamlanmış sayılamaz:
- Kamera açılıyor, izin ver/ret/permanent ret akışları çalışıyor.
- Photo Picker açılıyor, seçim/iptal çalışıyor.
- Seçilen fotoğraf preview'a geliyor, silinip değiştirilebiliyor.
- Kahve/El kalite kontrolü gerçek görüntüyle çalışıyor.
- Tarot kartlarına dokunma selection state'ini değiştiriyor ve duplicate üretmiyor.
- Chat input, send, Qwen generation, loading, retry/cancel ve persistence çalışıyor.
- Free/Premium reklam kapıları doğru uygulanıyor.
- Premium satın alma + otomatik restore + manuel restore çalışıyor.
- Premium aktifken hiçbir reklam request/container/gösterimi yok.
- Uygulama içi kullanıcı metinlerinde Premium özelliği için `PRO/Pro` bulunmuyor.
- Hiçbir mockup runtime'da bütün ekranı kaplayan statik UI olarak kullanılmıyor.

## 10. Tasarım referansları
`design_refs/ui/` altındaki JPG dosyaları bu ekranların görsel dilini, yerleşim fikrini, tipografi/ışık/kompozisyon yönünü anlatır. Uygulama bunları birebir screenshot olarak kullanmak yerine mevcut gerçek asset'ler + Flutter component'leriyle yeniden kurar.

Eski JPG mockup içinde `PRO/Pro` yazısı görünüyorsa bu copy **legacy/geçersiz** kabul edilir; runtime'da ve yeni görsel referanslarda yalnız `Premium` kullanılacaktır.
