# LP FAL — V1 Ürün ve Teknik Şartnamesi

**Repo:** `isarpsiyahofficial/LPFAL`  
**Platform:** Android (APK test / AAB Google Play)  
**Ürün adı:** **LP FAL**  
**Durum:** V1 kapsamı kilitli  
**Normatif ekler:** `V1_RELEASE_GATES.md`, `MONETIZATION_V1.md`

---

## 1. Ürün hedefi

LP FAL; **Kahve Falı, Tarot Falı ve El Falı** sunan, yorum ve fal sohbetini mümkün olduğunca cihaz üzerinde çalışan yerel Qwen modeliyle üreten, reklam destekli ve aylık reklamsız Premium seçeneği bulunan Android uygulamasıdır.

Ana hedefler:
- Hızlı, stabil ve sade kullanım.
- Yapay zekânın yalnız fal analizi/yorumu ve aktif fala bağlı sohbet için kullanılması.
- Fotoğraf, prompt ve sohbet verisinin AI inference amacıyla sunucuya gönderilmemesi.
- Ücretsiz sürümde güçlü fakat politika uyumlu monetizasyon.
- Premium'da tamamen reklamsız kullanım.
- Kesin gelecek iddiası, teşhis veya hayat kararı yönlendiren tavsiye üretilmemesi.
- V1 tamamlanana kadar kapsamın yeniden büyütülmemesi.

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
8. Premium / aylık reklamsız abonelik.
9. Profil / temel ayarlar.
10. AdMob reklam sistemi.
11. Yerel Qwen AI motoru.

### V1'e eklenmeyecekler
- Burç / astroloji.
- Rüya tabiri.
- Numeroloji.
- Sosyal ağ / kullanıcılar arası mesajlaşma.
- Coin/kredi sistemi.
- Tek seferlik Premium satın alma.
- Çok katmanlı VIP paketler.
- Cloudflare AI / R2 / harici inference sunucusu.
- Genel amaçlı AI asistanı.
- Parmak iziyle kimlik doğrulama veya biyometrik kimlik sistemi.

---

## 3. Marka ve görsel kimlik

### 3.1 İsim ve logo
Uygulamanın görünen adı her yerde **LP FAL** olacaktır.

Launcher, splash ve uygulama içi marka logosu MIZANGLOBAL reposundaki mevcut marka asset'iyle aynı olacaktır:

`MIZANGLOBAL/assets/brand/lefferion-prime-logo.png`

Logo yeniden tasarlanmayacak; yalnız Android adaptive icon gereksinimleri için aynı geometriden teknik varyasyonlar hazırlanabilir.

### 3.2 UI yönü
- Açık, sıcak ve premium görünüm.
- Koyu mor/siyah/neon “AI uygulaması” klişesi yok.
- Ana palet: `#FAF7F1`, `#EEE5D8`, `#5C4033`, `#B58B5A`, `#202020`.
- Tipografi temiz ve modern.
- Efekt, gradient ve gölge ölçülü.
- Telefon/tablet/BlueStacks responsive.

### 3.3 Görsel asset politikası
- Uygulama içi dekoratif kahve, tarot ve el görselleri AI ile üretilmeyecek.
- Ticari kullanıma uygun gerçek fotoğraf, kaliteli lisanslı illüstrasyon veya public-domain asset kullanılacak.
- Görseller hotlink edilmeyecek; yerel JPEG/WebP/PNG asset olarak paketlenecek.
- Kaynak ve lisanslar `assets/licenses/ASSET_LICENSES.md` içinde tutulacak.
- Tarot için 78 kartın tamamı tek ve tutarlı yüksek kaliteli desteden gelecek.

---

## 4. Hukuki / güvenli yorumlama ilkesi — tüm fal türleri için zorunlu

LP FAL kendisini **eğlence ve kişisel yorum uygulaması** olarak konumlandırır. Fal çıktıları bilimsel gerçek, teşhis, hukuki görüş, yatırım görüşü veya kesin gelecek tahmini olarak sunulmaz.

### 4.1 Kesinlik yasağı
AI şu biçimde konuşmayacaktır:
- `Kesin olacak.`
- `Şu tarihte başına gelecek.`
- `Kesin ayrılacaksın / evleneceksin.`
- `Şu hastalığın var.`
- `Hamilesin / hamile kalacaksın.`
- `Davayı kazanacaksın.`
- `Bu yatırımı yap, para kazanacaksın.`

Tercih edilen dil:
- `Geleneksel fal yorumlarında ... ile ilişkilendirilebilir.`
- `Bu görünüm ... temasını çağrıştırabilir.`
- `Sembolik olarak ... şeklinde yorumlanabilir.`

### 4.2 Tavsiye / karar yönlendirme yasağı
AI kullanıcının önemli hayat kararını yönlendiren emir veya profesyonel tavsiye vermeyecektir. Özellikle:
- ilişkiyi bitir / evlen / boşan,
- işini bırak,
- kredi çek / yatırım yap / bahis oyna,
- tedaviyi bırak / ilaç kullan,
- hukuki işlem başlat / başlatma
şeklinde yönlendirme yapılmaz.

Kullanıcı tavsiye isterse model fal bağlamında **sembolik ve karar vermeyen** yorum üretir; profesyonel tavsiye yerine geçmediğini belirtir.

### 4.3 Yüksek riskli içerik filtresi
Kesin veya korkutucu biçimde üretilemeyecek başlıklar:
- ölüm / yaşam süresi,
- ciddi hastalık veya sağlık teşhisi,
- hamilelik/doğurganlık kesinliği,
- suçluluk veya hukuki hüküm,
- garantili finansal sonuç,
- kendine zarar verme teşviki,
- şiddet/suç yönlendirmesi,
- nefret/taciz/cinsel uygunsuz içerik.

### 4.4 Sonuç ekranı bildirimi
Fal sonuçlarında görünür fakat rahatsız etmeyen sabit bilgi bulunur:

**“LP FAL yorumları eğlence ve kişisel yorum amaçlıdır; tıbbi, hukuki, finansal veya profesyonel tavsiye değildir.”**

---

## 5. Dashboard

### Üst alan
- Lefferion Prime/Mizan logosu.
- `LP FAL` adı.
- Profil/Ayarlar erişimi.

### Ana fal kartları
Mobilde önerilen hiyerarşi:
1. Büyük hero kart: **Kahve Falı** — `Falına Bak`.
2. **Tarot Falı** — `Kartlarını Seç`.
3. **El Falı** — `Avucunu Yorumla`.

Dar telefonda kartlar dikey; yeterli genişlikte Tarot + El Falı iki kolon olabilir. Tablet/BlueStacks'ta responsive grid kullanılabilir.

Aktif/son fal varsa `Falına Devam Et / Sohbete Dön` kartı gösterilir.

Alt navigasyon:
- Ana Sayfa
- Fallarım
- Premium
- Profil

---

## 6. Ortak fotoğraf alma ve gizlilik altyapısı

Kahve ve El Falı aynı güvenli media katmanını kullanacaktır.

- Kamera ve System Photo Picker desteklenecek.
- Geniş galeri/storage izni mümkün olduğunca istenmeyecek.
- Kamera izni yalnız kullanıcı kamera akışını başlattığında istenecek.
- EXIF/orientation normalize edilecek.
- Görseller app-private geçici alanda işlenecek.
- Production loglarına fotoğraf yolu, prompt, chat veya structured-analysis yazılmayacak.
- Ham fotoğraflar varsayılan olarak analiz tamamlandıktan sonra silinecek.
- Kullanıcı fotoğrafları varsayılan olarak model eğitimi/fine-tune için kullanılmayacak.
- Android Auto Backup ile ham fotoğraf/AI cache/model izinsiz buluta taşınmayacak.

---

## 7. Kahve Falı

### 7.1 Girdi
- 2–3 fincan fotoğrafı; 3 fotoğraf önerilir.
- Blur, exposure, çözünürlük, fincan içi görünürlük ve yanlış görsel kontrolü.
- Kalitesiz fotoğrafta AI inference başlamadan retry.

### 7.2 Analiz pipeline'ı
1. Normalize.
2. Fincanı ve kullanılabilir alanı tespit et.
3. Ağız / orta / dip / kulp çevresi crop'ları.
4. Her görüntüde structured visual analysis.
5. Çoklu görüntüyü tek `merged cup analysis` altında birleştir.
6. Duplicate sembolleri tekilleştir; çelişenleri uncertain/ele.
7. Confidence sinyalini validation setiyle kalibre et.
8. Yalnız merged structured analysis üzerinden fal metni üret.

Örnek ham veri:
```json
{
  "region": "upper_left",
  "shape": "bird_like",
  "raw_confidence": 0.82,
  "cross_view_agreement": true
}
```

Modelin kendi verdiği confidence istatistiksel gerçek kabul edilmez.

### 7.3 Sonuç
- Genel Yorum
- Aşk
- İş / Para
- Yol / Değişim

Yorumlar Bölüm 4 güvenlik kurallarına tabidir.

---

## 8. Tarot Falı

### 8.1 Deste ve draw engine
- 78 kartlık tek ve tutarlı deste.
- Her kartın ID, Türkçe adı, asset path'i ve temel metadata'sı bulunur.
- Aynı açılım içinde aynı kart iki kere çekilemez.
- Draw uygulama tarafından yapılır; Qwen kart seçmez.
- Güvenilir RNG (`Random.secure()` veya eşdeğeri) kullanılır.
- Düz/ters durumu draw'dan ayrı belirlenir ve merkezi config ile yönetilir.

### 8.2 Açılım türleri
- Tek Kart.
- 3 Kart: Geçmiş / Şimdi / Gelecek.
- 5 Kart: Geçmiş / Şimdi / Gizli Etki / Yakın Gelecek / Sonuç-Tema.

### 8.3 Kullanıcı deneyimi
- Kullanıcı soru yazabilir; opsiyoneldir.
- Deste karıştırma animasyonu hafif ve hızlıdır.
- Ekrandaki kapalı kartlardan seçim hissi verilir.
- Uygulama seçilen slotu gerçek 78 kartlık engine'deki draw sonucu ile eşleştirir.
- Kart açıldığında adı, pozisyonu ve düz/ters state'i gösterilir.

### 8.4 AI yorumu
Qwen'e görsel tanıma yaptırılmaz. Structured input verilir:
- soru,
- açılım türü,
- kart ID/ad,
- pozisyon,
- upright/reversed.

Qwen:
- listede olmayan kart ekleyemez,
- kart state'ini değiştiremez,
- kartları tek tek + kombinasyon halinde yorumlar,
- Bölüm 4 güvenlik kurallarına uyar.

---

## 9. El Falı

### 9.1 Ürün konumu
El Falı, **geleneksel palmistry/el falı sembolizmini eğlence amaçlı yorumlayan** görsel modüldür. Bilimsel kişilik analizi, sağlık analizi veya biyometrik kimlik sistemi değildir.

### 9.2 Fotoğraf girişi
- 1–2 net avuç içi fotoğrafı.
- Kullanıcıdan avuç içini açık, iyi ışıkta ve mümkün olduğunca düz göstermesi istenir.
- Blur, düşük çözünürlük, aşırı gölge/ışık, el/avuç görünmeme ve ciddi oklüzyon kontrol edilir.
- Gerekirse kullanıcıdan yeniden fotoğraf istenir.

### 9.3 Görsel analiz pipeline'ı
1. Görüntüyü normalize et.
2. El/avuç içi varlığını doğrula.
3. Avuç bölgesini crop et.
4. Yalnız görünür çizgi/bölge özelliklerini structured olarak çıkar.
5. Geleneksel palmistry terminolojisiyle aday öğeleri işaretle:
   - kalp çizgisi,
   - baş çizgisi,
   - yaşam çizgisi,
   - kader çizgisi yalnız görünüyorsa,
   - belirgin kesişim/dallanma/yoğunluk gibi genel görünür yapı.
6. Görünmeyen çizgi için veri uydurma.
7. Confidence kalibrasyonu ve negatif örnek testi uygula.
8. İkinci aşamada yalnız structured analysis üzerinden sembolik yorum üret.

### 9.4 El Falı için kesin yasaklar
- Parmak izi template'i çıkarma veya saklama.
- Kullanıcıyı benzersiz tanıma/doğrulama.
- El fotoğrafından kimlik eşleştirme.
- Sağlık/hastalık teşhisi.
- Yaşam süresi/ölüm tarihi çıkarımı.
- Hamilelik/doğurganlık çıkarımı.
- Irk/etnik köken, din, siyasi görüş, cinsel yönelim gibi hassas özellik çıkarımı.
- Yaş/cinsiyet gibi gereksiz profil tahmini.
- `Elin böyle, o yüzden kesin şu kişiliğe sahipsin` şeklinde deterministik kişilik hükmü.

### 9.5 El Falı sonucu
Önerilen başlıklar:
- Genel Enerji / Tema
- Duygusal Alan
- Zihin / Yaklaşım
- Değişim / Yol Teması

Her bölüm `geleneksel el falı yorumlarında`, `sembolik olarak`, `çağrıştırabilir` dilini kullanır.

---

## 10. Ortak AI güvenilirlik ve kalite sistemi

### Kahve
Release öncesi en az 100 gerçek fincan fotoğrafı/grubu.
- İnsan etiketli görünür bölgeler/semboller.
- Negatif örnekler.
- Hedef high-confidence precision ≥ %80.
- Hedef high-confidence false-positive ≤ %15.
- Multi-view fusion testi.

### El
Release öncesi en az 100 gerçek avuç içi fotoğrafı/grubu.
- İnsan tarafından görünür çizgi/bölge etiketleri.
- Farklı ışık, ten tonu, kamera kalitesi ve el pozisyonu çeşitliliği.
- Çizginin görünmediği negatif örnekler.
- Modelin görünmeyen çizgi uydurmaması release gate.
- Confidence threshold'ları gerçek validation sonuçlarına göre kalibre edilir.

### Türkçe yorum kalitesi
- Kahve + Tarot + El + Chat için sabit regresyon prompt seti.
- Kesinlik/tavsiye yasağı otomatik test edilir.
- Model/prompt/runtime değişince regresyon yeniden koşar.

---

## 11. Fal Sohbeti

- Yalnız aktif Kahve/Tarot/El falının context'iyle konuşur.
- Genel amaçlı chatbot değildir.
- Kahvede merged analysis + sonuç.
- Tarotta kart metadata + sonuç.
- Elde palm structured analysis + sonuç.
- Yeni fal yeni conversation context oluşturur.
- Context büyürse yerel özetleme yapılır.
- Kod, haber, ödev vb. fal dışı istekler reddedilir.
- Sohbet de Bölüm 4 kesinlik/tavsiye kurallarına tabidir.

---

## 12. Yerel AI — Qwen

Ana hedef **Qwen3.5-0.8B** ailesinin lisans açısından uygun resmi ağırlıklarından hazırlanmış mobil/quantized sürümdür.

- Resmi model/revision sabitlenir.
- Kahve ve El görsellerini işleyebilmek için seçilen runtime/model paketinin görsel input desteği gerçek cihazda doğrulanır.
- Q4 sınıfı ilk aday; gerçek benchmark'a göre değişebilir.
- Model, projector/vision bileşeni varsa ayrı SHA-256 ile sürümlenir.
- Prompt sürümleri ayrı tutulur: coffee, tarot, palm, chat, safety.
- AI inference ana UI thread'ini bloke etmez.

### Ağ
AI inference için:
- Cloudflare yok.
- Harici inference API yok.
- Fotoğraf/prompt/chat sunucuya gönderilmez.

### Debug/test model
- PC'den ADB/local kurulum.
- Google Play zorunlu değil.
- SHA-256 bütünlük kontrolü.

### Release model
- Model base APK içine gömülmez.
- Release tarihinde uygun Google Play on-device AI / asset delivery yöntemi yeniden doğrulanır.
- Model geldikten sonra inference offline yapılır.

### Cihaz hedefi
- 4 GB RAM: destek hedefi, agresif optimizasyon ve gerçek test.
- 6 GB RAM: ana hedef.
- 8 GB+: rahat hedef.
- Unsupported/OOM durumunda crash yerine açıklayıcı state.

---

## 13. Yerel veri ve gizlilik

Cihazda tutulabilecekler:
- fal türü,
- structured analysis,
- fal metni,
- tarot kart metadata,
- sohbet geçmişi,
- kullanıcı tercihleri,
- model durumu.

Ham kahve/el fotoğrafları varsayılan kalıcı kayıt değildir.

Kullanıcı:
- tek falı silebilir,
- tüm geçmişi silebilir,
- uygulama verisini sıfırlayabilir.

AI çıktısı raporlama gibi kullanıcı tarafından açıkça başlatılan ağ işlemleri Privacy Policy/Data Safety'de açıklanır.

---

## 14. Monetizasyon

Ayrıntılı ve normatif kurallar `MONETIZATION_V1.md` içindedir.

### Ücretsiz kullanıcı
- Kahve tam sonucu: **2 Rewarded Ad**.
- Tarot tam yorumu: **2 Rewarded Ad**.
- El Falı tam yorumu: **2 Rewarded Ad**.
- Fal sohbeti yeni mesaj paketi: **2 Rewarded Ad**.
- Reward ilerlemesi `0/2 → 1/2 → 2/2`.
- Tek reklam ödül vermez.

### Süreli interstitial
- `timedInterstitialEligibilitySeconds = 90`.
- 90 saniye reklamı otomatik açmaz; yalnız uygunluk oluşturur.
- Reklam sonraki güvenli/doğal ekran geçişinde gösterilir.
- Analiz, fotoğraf, tarot/el seçim akışı, sonuç okuma, chat yazma veya rewarded sırasında gösterilmez.

### Banner
- Telefon: güvenli yüzeylerde tek anchored adaptive banner.
- Tablet/BlueStacks: gerekirse tek sağ/sol rail/banner kolonu.
- Fal analiz/chat/çekim/seçim ekranlarında banner yok.

### App Open
- Kontrollü, diğer full-screen reklamlarla çakışmayan kullanım.

---

## 15. Premium

V1 Premium: **aylık otomatik yenilenen reklamsız kullanım**.

Premium aktifken:
- Rewarded yok.
- Timed interstitial yok.
- App Open yok.
- Banner/native yok.
- Ad request mümkün olduğunca oluşturulmaz.
- Kahve/Tarot/El sonuçları reklam beklemeden açılır.
- Fal sohbeti reklam paketi beklemez.

Billing lifecycle: purchase, pending, restore, renewal, grace period, account hold, expiry/cancel ve process-death recovery test edilir.

---

## 16. Responsive ve erişilebilirlik

- 360dp küçük telefondan tablet/BlueStacks'a responsive.
- Safe area/notch/gesture navigation.
- TalkBack semantics.
- Kritik dokunma hedefleri mümkün olduğunca ≥48dp.
- Font scaling ile CTA/sonuç taşmamalı.
- Banner ile CTA/navigation/chat input arasında güvenli ayrım.

---

## 17. Test ve release zorunlulukları

### Fonksiyonel
- Dashboard ve üç fal türü.
- Kamera/Photo Picker.
- Kahve multi-view pipeline.
- Tarot 78 kart draw/mapping.
- El görüntü kalite + palm pipeline.
- Ortak sonuç/chat.
- History/silme.
- 2-ad reward akışı.
- 90 sn timed eligibility.
- Banner responsive placement.
- Premium purchase/restore/lifecycle.

### AI / safety
- 100+ fincan validation.
- 100+ avuç validation.
- Türkçe kalite regresyonu.
- Kesinlik/tavsiye ihlal testi.
- Fal dışı scope rejection.
- Prompt injection.
- Hassas özellik/biometric inference yasağı testi.

### Performans
- 4/6/8 GB cihaz sınıfları.
- 20+ ardışık inference stress.
- Uzun chat memory leak.
- background/foreground.
- low-memory recovery.
- thermal gözlem.

### Ağ/gizlilik
- AI fotoğraf/prompt/chat outbound request yok.
- Ham fotoğraf cleanup.
- Production log sızıntısı yok.
- Auto Backup exclusions.

---

## 18. V1 final kabul kriterleri

Aşağıdakilerden biri eksikse V1 final değildir:

- [ ] İsim her yerde LP FAL.
- [ ] Mizan/Lefferion Prime logo asset'i doğru.
- [ ] Açık/krem onaylı dashboard ve 3 ana fal kartı tamam.
- [ ] Uygulama içi dekoratif görseller lisanslı ve AI üretimi değil.
- [ ] Kahve Falı uçtan uca çalışıyor ve merged structured analysis kullanıyor.
- [ ] Tarot 1/3/5 kart akışı ve 78 kart mapping testi geçiyor.
- [ ] El Falı uçtan uca çalışıyor ve yalnız görünür avuç özelliklerini yorumluyor.
- [ ] El Falı biyometrik kimlik/parmak izi sistemi oluşturmuyor.
- [ ] Kahve ve El confidence değerleri validation ile kalibre edilmiş.
- [ ] AI kesin gelecek iddiası veya hayat kararı yönlendiren tavsiye üretmiyor.
- [ ] Sağlık/ölüm/hamilelik/hukuk/garantili finans gibi riskli kesin çıkarımlar engelli.
- [ ] Fal sohbeti yalnız aktif fal bağlamında.
- [ ] Qwen lokal çalışıyor; inference verisi sunucuya gitmiyor.
- [ ] Free kullanıcı için her reward unlock 2 başarılı Rewarded Ad gerektiriyor.
- [ ] Timed interstitial uygunluğu 90 saniye aktif kullanım ve güvenli geçiş kuralına uyuyor.
- [ ] Banner yerleşimi telefon/tablet/BlueStacks'ta yanlış tıklama riski yaratmıyor.
- [ ] Premium aktifken bütün reklam türleri kapalı.
- [ ] Geçmiş yerelde tutuluyor ve silinebiliyor.
- [ ] 4/6/8 GB QA tamam.
- [ ] Privacy/Data Safety/lisans/AI reporting gereksinimleri tamam.
- [ ] Signed APK ve AAB temiz build/install testini geçiyor.

**V1 checklist'te açık madde varken final etiketi verilemez.**