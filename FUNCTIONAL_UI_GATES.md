# LP FAL — Functional UI Gates

Bu dosya, `design_refs/ui/` altındaki üretilmiş ekran mockup'larının uygulamada nasıl hayata geçirileceğini bağlayıcı olarak tanımlar. Mockup dosyaları **yalnız görsel referanstır**; ekranın tamamı runtime'da resim olarak gösterilemez. Metinler, butonlar, kartlar, navigasyon, progress, chat balonları ve etkileşimler Flutter widget'larıyla gerçek işlevli olarak uygulanacaktır.

## 1. V1 kapsamı korunacak
- V1 gerçek modüller: **Kahve Falı, Tarot Falı, Rüya Tabiri, El Falı**, aktif fal/rüyaya bağlı AI sohbeti, geçmiş, Premium, profil/ayarlar, reklamlar ve cihaz içi Qwen.
- Rüya Tabiri konusunda `DREAM_INTERPRETATION_SPEC.md` normatiftir.
- Uygulama genelinde `COMPLIANCE_BY_DESIGN.md` zorunludur.
- Mockup'larda görünen Günlük Yorum, Aşk Uyumu, Kariyer modülü vb. V1 kapsamına otomatik olarak girmez.
- Premium ürün modeli ve kullanıcıya görünen adlandırmada `PRODUCT_MODEL_V1.md` ana kaynaktır.

## 2. Gerçek AI sohbeti — statik görsel yasak
- Sohbet ekranı gerçek, kaydırılabilir mesaj listesi içerecek.
- Kullanıcı metin alanına yazacak ve gönder butonuyla mesaj gönderecek.
- Kullanıcı ve Qwen mesajları ayrı balonlarda, doğru sırayla gösterilecek.
- Qwen yanıt üretirken loading/generating durumu gösterilecek; UI ana thread'i bloklanmayacak.
- Üretim iptal/retry akışı desteklenecek.
- Sohbet yalnız aktif fal/rüya bağlamında çalışacak; genel amaçlı asistan olmayacak.
- Kahve bağlamı: merged structured cup analysis + final result + sınırlandırılmış sohbet geçmişi.
- Tarot bağlamı: seçilen kart metadata'ları + pozisyonlar + yönler + final yorum + sınırlandırılmış geçmiş.
- Rüya bağlamı: user dream input/güvenli local summary + structured dream extraction + final symbolic interpretation + sınırlandırılmış geçmiş.
- El Falı bağlamı: structured palm analysis + final result + sınırlandırılmış geçmiş.
- Sohbet ve prompt cihaz içi Qwen ile işlenecek; AI inference için sunucuya gönderilmeyecek.
- Free kullanıcı: ilk takip sorusu ücretsiz; ardından her 3 kullanıcı mesajı paketi için 2 Rewarded Ad. Premium: reklam kapısı yok.
- Chat geçmişi yerel saklanacak ve yeni fal/rüya başladığında yeni conversation context açılacak.
- Sağlık, psikoloji, hukuk, finans, din, ölüm, hamilelik vb. alanlarda kesinlik veya yönlendirici tavsiye üretilmeyecek.
- Tüm AI mesajlarında erişilebilir in-app `Bildir`/flag akışı bulunacak.

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
- AI analiz ekranı gerçek inference durumunu gösterecek; sahte yüzde ilerleme kullanılmayacak.

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

## 7. Rüya Tabiri ekranı
- Dashboard'da **Rüya Tabiri** gerçek route/card olarak bulunacak.
- `Rüyanı Anlat`/`Rüyanı Yorumla` CTA gerçek rüya giriş ekranını açacak.
- Rüya giriş alanı gerçek çok satırlı `TextField/TextFormField` olacak.
- Kullanıcı rüyasını yazmadan `Yorumla` aktif olmayacak.
- Opsiyonel duygu seçimi gerçek state olacak; zorunlu olmayacak.
- Rüya metni kullanıcı göndermeden AI pipeline'a aktarılmayacak.
- AI ilk aşamada structured dream extraction yapacak; kullanıcının yazmadığı sembol/sahne UI'a eklenmeyecek.
- Sahte yüzde ilerleme yok.
- Sonuç ekranı gerçek scrollable içerik olacak.
- `Tavsiye`, `Ne yapmalısın`, `Karar` bölümü bulunmayacak.
- `Rüya hakkında sohbet et` gerçek aktif-rüya chat context'i açacak.
- Sonuç ve AI chat mesajlarında `Bildir` aksiyonu erişilebilir olacak.
- Rüya metni production log/analytics'e yazılmayacak.

## 8. Navigasyon, Premium ve butonlar
- Dashboard Kahve/Tarot/Rüya/El kartları gerçek route/navigation butonlarıdır.
- Bottom navigation gerçek sayfa/state navigasyonu yapar.
- Premium butonu Google Play tek seferlik satın alma ekranına/akışına bağlanır.
- Kullanıcıya görünen plan adı yalnız `LP FAL Premium` / `Premium` olacaktır.
- `PRO`, `Pro`, `Ömür Boyu Premium` ve `Lifetime Premium` kullanıcı metni olarak kullanılmaz.
- Satın alma açıklaması: `Tek seferlik satın alım · Abonelik değildir.`
- Satın alma CTA'sı: `Premium'a Geç` veya `Premium'u Aç`.
- **Restore kullanıcıya görünmez. Premium veya Ayarlar ekranında restore/geri yükleme butonu bulunmaz.**
- `Satın Alımı Geri Yükle`, `Restore`, `Geri Yükle` gibi teknik restore metinleri kullanıcı UI'ında gösterilmez.
- Restore açılış/resume/internet geri gelişi/clean install/yeni cihaz akışlarında arka planda sessiz ve otomatik çalışır.
- Geri, gönder, kamera, galeri, kart seç, rüya yorumla, analiz, retry ve report kontrollerinin tamamı gerçek callback/state'e bağlıdır.
- Görsel üstüne basılmış sahte UI metni/button kabul edilmez.

## 9. Global compliance UI
- Kahve/Tarot/Rüya/El sonuçlarında görünür kısa entertainment disclaimer bulunur.
- Disclaimer tıbbi, psikolojik, hukuki, finansal, dini veya diğer profesyonel danışmanlık ve kesin gelecek iddiası olmadığını açıklar.
- Disclaimer safety filtresinin yerine geçmez.
- AI içeriği kullanıcı tarafından uygulamadan çıkmadan report/flag edilebilir.
- Health/self-harm/violence gibi high-risk state oluşursa normal fal/rüya UI akışı yerine güvenli response state gösterilir.
- Dark pattern, sahte geri sayım veya AI sonucunu sistem uyarısı gibi gösterme yok.

## 10. Responsive ve erişilebilirlik
- Telefon 360dp'den büyük telefon/tablet/BlueStacks'a kadar overflow olmamalı.
- Safe area, notch, gesture navigation desteklenmeli.
- Kritik touch target >=48dp.
- TalkBack için anlamlı semantic label bulunmalı.
- Font scaling kritik ekranları bozmamalı.

## 11. Kabul testi — bloklayıcı
Aşağıdakiler manuel/otomatik test edilmeden ilgili UI fazı tamamlanmış sayılamaz:
- Kamera açılıyor, izin ver/ret/permanent ret akışları çalışıyor.
- Photo Picker açılıyor, seçim/iptal çalışıyor.
- Seçilen fotoğraf preview'a geliyor, silinip değiştirilebiliyor.
- Kahve/El kalite kontrolü gerçek görüntüyle çalışıyor.
- Tarot kartlarına dokunma selection state'ini değiştiriyor ve duplicate üretmiyor.
- Rüya text input/gönder/structured extraction/result/persistence çalışıyor.
- Rüya AI kullanıcı inputunda olmayan ayrıntı uydurmuyor.
- Chat input, send, Qwen generation, loading, retry/cancel ve persistence dört modülde çalışıyor.
- AI report/flag sonucu ve chat mesajından çalışıyor.
- Free/Premium reklam kapıları doğru uygulanıyor.
- Rüya tam sonucu Free kullanıcıda 2 Rewarded gerektiriyor.
- Premium satın alma çalışıyor.
- Clean install/yeni cihaz/app resume/internet dönüşünde **otomatik ve görünmez restore** çalışıyor.
- Kullanıcı UI'ında restore/geri yükleme butonu veya teknik restore metni bulunmuyor.
- Premium aktifken hiçbir reklam request/container/gösterimi yok.
- Uygulama içi kullanıcı metinlerinde Premium özelliği için `PRO/Pro` bulunmuyor.
- `COMPLIANCE_BY_DESIGN.md` high-risk ve misleading-claim testleri geçiyor.
- Hiçbir mockup runtime'da bütün ekranı kaplayan statik UI olarak kullanılmıyor.

## 12. Tasarım referansları
`design_refs/ui/` altındaki JPG dosyaları ekranların görsel dilini, yerleşim fikrini, tipografi/ışık/kompozisyon yönünü anlatır. Uygulama bunları birebir screenshot olarak kullanmak yerine mevcut gerçek asset'ler + Flutter component'leriyle yeniden kurar.

Eski JPG mockup içinde legacy Premium/restore copy görünüyorsa bu copy **geçersiz** kabul edilir; runtime'da ve yeni görsel referanslarda kullanılmaz.
