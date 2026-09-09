# LP FAL — V1 Ürün ve Teknik Şartnamesi

**Repo:** `isarpsiyahofficial/LPFAL`  
**Platform:** Android (APK test / AAB Google Play)  
**Ürün adı:** **LP FAL**  
**Durum:** V1 kapsamı kilitli  
**Normatif ekler:** `PRODUCT_MODEL_V1.md`, `V1_RELEASE_GATES.md`, `MONETIZATION_V1.md`, `BILLING_RESTORE_SPEC.md`, `FUNCTIONAL_UI_GATES.md`

> Premium ürün tipi ve kullanıcıya görünen Premium/PRO adlandırması konusunda `PRODUCT_MODEL_V1.md` ana kaynaktır.

---

## 1. Ürün hedefi

LP FAL; **Kahve Falı, Tarot Falı ve El Falı** sunan, yorum ve fala bağlı sohbeti mümkün olduğunca cihaz üzerinde çalışan yerel Qwen modeliyle üreten, reklam destekli ve tek seferlik **LP FAL Premium** satın alımı bulunan Android uygulamasıdır.

Ana hedefler:
- Hızlı, stabil ve sade kullanım.
- AI yalnız fal analizi/yorumu ve aktif fala bağlı sohbet için kullanılır.
- Fotoğraf, prompt ve sohbet AI inference amacıyla sunucuya gönderilmez.
- Free sürüm güçlü fakat politika uyumlu reklam modeli kullanır.
- Premium tamamen reklamsızdır.
- Premium abonelik değildir; tek seferlik Google Play satın alımıdır.
- Kesin gelecek iddiası, teşhis veya hayat kararı yönlendiren tavsiye üretilmez.
- V1 tamamlanana kadar kapsam yeniden büyütülmez.

---

## 2. V1 kapsam kilidi

V1 ana modülleri:
1. Dashboard.
2. Kahve Falı.
3. Tarot Falı.
4. El Falı.
5. Fal sonucu.
6. Aktif fala bağlı fal sohbeti.
7. Geçmiş Fallar.
8. **LP FAL Premium — tek seferlik satın alma / restore.**
9. Profil / temel ayarlar.
10. AdMob reklam sistemi.
11. Yerel Qwen AI motoru.

V1'e eklenmeyecekler:
- Burç / astroloji.
- Rüya tabiri.
- Numeroloji.
- Sosyal ağ / kullanıcılar arası mesajlaşma.
- Coin/kredi sistemi.
- Aylık/yıllık/otomatik yenilenen abonelik.
- Çok katmanlı VIP paketler.
- Cloudflare AI / R2 / harici inference sunucusu.
- Genel amaçlı AI asistanı.
- Parmak iziyle kimlik doğrulama veya biyometrik kimlik sistemi.

---

## 3. Marka ve isim standardı

### 3.1 Uygulama adı ve logo
- Görünen ad her yerde **LP FAL**.
- Launcher, splash ve uygulama içi marka logosu MIZANGLOBAL reposundaki mevcut Lefferion Prime logo asset'inin aynı marka geometrisini kullanır.
- Logo yeniden markalanmaz; yalnız Android adaptive icon teknik varyasyonları hazırlanabilir.

### 3.2 Premium isim standardı
Premium özelliği için kullanıcıya görünen tek terim **Premium**'dur.

Doğru:
- `LP FAL Premium`
- `Premium`
- `Premium'a Geç`
- `Premium Aktif`
- `Satın Alımı Geri Yükle`

Kullanılmayacak:
- `PRO`
- `Pro`
- `LP FAL PRO`
- `Ömür Boyu Premium`
- `Lifetime Premium`
- `Aylık Premium`

Satın alma açıklaması:
**Tek seferlik satın alım · Abonelik değildir.**

`Lefferion Prime` marka adındaki `Prime`, Premium ürün etiketi değildir.

---

## 4. UI / görsel yön

- Açık, sıcak ve premium görünüm.
- Koyu mor/siyah/neon “AI uygulaması” klişesi yok.
- Ana palet: `#FAF7F1`, `#EEE5D8`, `#5C4033`, `#B58B5A`, `#202020`.
- Tipografi temiz ve modern.
- Efekt/gradient/gölge ölçülü.
- Telefon/tablet/BlueStacks responsive.
- `design_refs/ui/` JPG'leri yalnız görsel referanstır; runtime ekranı screenshot olarak kullanılmaz.
- Eski mockup içinde `PRO/Pro` geçerse bu metin legacy/geçersizdir; runtime copy yalnız `Premium` olur.

### 4.1 Görsel asset politikası
- Statik/dekoratif görseller repo içinde yerel asset olarak tutulur.
- Hotlink yok.
- Ticari kullanım lisansları kayıt altına alınır.
- Tarot 78 kart seti tek ve tutarlı desteden gelir.
- Uygulama içi buton/başlık/metin görselin içine gömülmek yerine Flutter widget olarak üretilir.

---

## 5. Hukuki / güvenli yorumlama ilkesi

LP FAL kendisini **eğlence ve kişisel yorum uygulaması** olarak konumlandırır. Fal çıktıları bilimsel gerçek, teşhis, hukuki görüş, yatırım görüşü veya kesin gelecek tahmini olarak sunulmaz.

AI kesin biçimde şunları söylemez:
- ölüm/yaşam süresi,
- ciddi hastalık/teşhis,
- hamilelik/doğurganlık kesinliği,
- hukuki sonuç garantisi,
- garantili finansal/bahis sonucu,
- önemli hayat kararını emreden yönlendirme.

Tercih edilen dil:
- `Geleneksel fal yorumlarında...`
- `Sembolik olarak...`
- `...çağrıştırabilir.`

Fal sonuçlarında görünür sabit bilgi:
**“LP FAL yorumları eğlence ve kişisel yorum amaçlıdır; tıbbi, hukuki, finansal veya profesyonel tavsiye değildir.”**

Safety yalnız prompt'a bırakılmaz; uygulama-level output kontrolü de bulunur.

---

## 6. Dashboard

Üst alan:
- Lefferion Prime logo.
- LP FAL adı.
- Profil/Ayarlar erişimi.

Ana kartlar:
1. Kahve Falı — `Falına Bak`.
2. Tarot Falı — `Kartlarını Seç`.
3. El Falı — `Avucunu Yorumla`.

Aktif/son fal varsa `Falına Devam Et / Sohbete Dön` kartı.

Bottom navigation:
- Ana Sayfa
- Fallarım
- Premium
- Profil

---

## 7. Ortak fotoğraf alma ve gizlilik altyapısı

Kahve + El Falı:
- Kamera + Android System Photo Picker.
- Geniş storage permission mümkün olduğunca yok.
- Kamera izni yalnız kullanıcı capture başlatınca.
- EXIF/orientation normalize.
- App-private temp storage.
- Blur/exposure/resolution/wrong-image kalite kontrolü.
- Production loglarında fotoğraf yolu/prompt/chat/structured-analysis yok.
- Ham fotoğraflar varsayılan inference sonrası silinir.
- Training/fine-tune için kullanıcı verisi varsayılan kullanılmaz.
- Auto Backup ile hassas raw/model/cache izinsiz buluta gitmez.

---

## 8. Kahve Falı

### Girdi
- 2–3 fincan fotoğrafı; 3 önerilir.
- Fincan içi görünürlük, blur, exposure, çözünürlük ve yanlış görsel kontrolü.
- Kalitesiz görsel inference başlamadan retry.

### Pipeline
1. Normalize.
2. Fincan/usable area tespiti.
3. Ağız/orta/dip/kulp çevresi crop.
4. Her görüntüde structured visual analysis.
5. Çoklu fotoğrafı tek `merged cup analysis` altında birleştir.
6. Duplicate sembolleri tekilleştir.
7. Çelişkili bulguları uncertain/ele.
8. Qwen self-confidence değerini gerçek olasılık kabul etme; validation ile kalibre et.
9. İkinci Qwen aşaması yalnız merged analysis üzerinden fal metni üretir.

Sonuç:
- Genel Yorum
- Aşk
- İş / Para
- Yol / Değişim

---

## 9. Tarot Falı

- 78 kartlık sabit ve tutarlı deste.
- Kart ID, Türkçe ad, asset path ve metadata.
- Draw uygulama tarafından güvenli RNG ile yapılır; Qwen kart seçmez.
- Aynı açılımda duplicate kart yok.
- Upright/reversed state draw'dan ayrı merkezi config.

Açılımlar:
- Tek Kart.
- 3 Kart: Geçmiş / Şimdi / Gelecek.
- 5 Kart: Geçmiş / Şimdi / Gizli Etki / Yakın Gelecek / Sonuç-Tema.

Qwen structured input alır:
- soru,
- açılım türü,
- kart ID/ad,
- pozisyon,
- upright/reversed.

Qwen listede olmayan kart ekleyemez veya state değiştiremez.

---

## 10. El Falı

- 1–2 net avuç içi fotoğrafı.
- El/avuç varlık kontrolü.
- Blur/resolution/exposure/occlusion kontrolü.
- Avuç crop/normalize.
- Yalnız görünür çizgi/bölge özelliklerini structured çıkar.
- Kalp/baş/yaşam/kader çizgisi yalnız görünüyorsa aday olarak işaretlenir.
- Görünmeyen öğe `not_visible/uncertain`.
- İkinci aşama yalnız structured analysis üzerinden sembolik yorum üretir.

Kesin yasak:
- fingerprint template,
- biyometrik kimlik/eşleştirme,
- sağlık/hastalık çıkarımı,
- ölüm/yaşam süresi çıkarımı,
- hamilelik/doğurganlık çıkarımı,
- ırk/etnik köken/din/siyasi görüş/cinsel yönelim çıkarımı,
- gereksiz yaş/cinsiyet tahmini,
- deterministik kişilik hükmü.

---

## 11. Fal Sohbeti

- Yalnız aktif Kahve/Tarot/El falı context'iyle konuşur.
- Genel amaçlı chatbot değildir.
- Yeni fal yeni conversation context oluşturur.
- Context büyürse yerel özetleme.
- Kod/haber/ödev gibi fal dışı istekler scope rejection alır.
- Chat çıktısı da safety filtresinden geçer.

Free:
- İlk takip sorusu ücretsiz.
- Sonrasında her 3 kullanıcı mesajı paketi = 2 Rewarded.

Premium:
- Reklam kapısı yok.

---

## 12. Yerel AI — Qwen

Ana hedef: resmi Qwen3.5-0.8B ailesinin uygun mobil/quantized sürümü.

- Resmi model/revision sabitlenir.
- Kahve + El için gerçek cihazda multimodal destek doğrulanır.
- Q4 ilk aday; benchmark'a göre değişebilir.
- Model/projector SHA-256 sürümlenir.
- Prompt sürümleri: coffee, tarot, palm, chat, safety.
- Inference UI thread'i bloklamaz.

AI inference için:
- Cloudflare yok.
- Harici inference API yok.
- Fotoğraf/prompt/chat sunucuya gönderilmez.

Debug:
- model PC'den ADB/local kurulabilir.
- Git repo history içine büyük model commit edilmez.

Release:
- model base APK içine gömülmez.
- release tarihindeki uygun Play model/asset delivery yöntemi doğrulanır.

---

## 13. Yerel veri

Cihazda tutulabilecekler:
- fal türü,
- structured analysis,
- final fal metni,
- tarot metadata,
- bağlı chat geçmişi,
- Premium entitlement cache/fingerprint metadata.

Ham fotoğraf varsayılan kalıcı saklanmaz.

---

## 14. Reklam sistemi — Free kullanıcı

`MONETIZATION_V1.md` ana kaynaktır.

- Kahve full result = 2 Rewarded.
- Tarot full result = 2 Rewarded.
- El full result = 2 Rewarded.
- Gated chat pack = 2 Rewarded.
- Timed interstitial eligibility = 90 saniye foreground aktif kullanım.
- Reklam yalnız güvenli/doğal geçişte.
- Phone: tek anchored adaptive banner güvenli ekranlarda.
- Tablet/BlueStacks: gerekirse tek side rail.
- App Open diğer full-screen akışlarla çakışmaz.

---

## 15. LP FAL Premium

### Ürün modeli
- Tek ürün: **LP FAL Premium**.
- Google Play üzerinden tek seferlik satın alma.
- Abonelik yok.
- Kullanıcıya `Ömür Boyu/Lifetime/PRO` plan adı gösterilmez.
- Açıklama: **Tek seferlik satın alım · Abonelik değildir.**
- Fiyat Google Play ürün bilgisinden gelir.

### Restore
- Purchase stream merkezi.
- Açılış/resume/internet geri gelişinde owned purchase sync.
- Clean install/yeni cihaz aynı Play hesabında restore.
- Manuel `Satın Alımı Geri Yükle`.
- Process-death recovery.
- Yerel boolean tek başına Premium vermez.

### Premium aktifken
- Rewarded yok.
- Timed interstitial yok.
- App Open yok.
- Banner/native yok.
- Yeni ad request yok.
- Loaded ads dispose.
- Empty ad container yok.
- Kahve/Tarot/El/chat doğrudan kullanılır.

---

## 16. Privacy / Play / ağ

İzin verilen network sınıfları:
- AdMob + UMP (Free kullanıcı),
- Google Play Billing / delivery,
- kullanıcı tarafından başlatılan AI report endpoint'i varsa yalnız minimum report payload.

AI inference network'e çıkmaz.

Privacy Policy:
- kalıcı HTTPS URL,
- gerçek SDK davranışına uygun Data Safety,
- raw fotoğraf default report payload'a eklenmez,
- production log redaction.

---

## 17. Responsive / accessibility

- 360dp küçük telefon → büyük telefon → tablet → BlueStacks.
- Safe area/notch/gesture navigation.
- Critical touch target tercihen >=48dp.
- TalkBack semantic labels.
- Font scaling overflow üretmez.
- Premium banner kaldırıldığında boş alan bırakılmaz.

---

## 18. Release kalite kapıları

Final için zorunlu:
- Kahve multi-view doğruluk QA.
- El Falı görünür çizgi/negatif/bias QA.
- Tarot 78 kart integrity.
- Qwen 4/6/8 GB gerçek cihaz performans testi.
- Network isolation.
- Sensitive production log yok.
- Free 2-Rewarded akışları.
- 90 saniye timed eligibility.
- Banner güvenli spacing.
- Premium tek seferlik purchase + restore testleri.
- Premium aktifken sıfır reklam request/container.
- UI içinde Premium ürünü için `PRO/Pro/Ömür Boyu/Lifetime/Aylık Premium` yok.
- APK/AAB clean install testi.

**Çatışma halinde Premium ürün modeli ve isim standardında `PRODUCT_MODEL_V1.md` uygulanır.**