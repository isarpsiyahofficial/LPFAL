# LP FAL — V1 Ürün ve Teknik Şartnamesi

**Repo:** `isarpsiyahofficial/LPFAL`  
**Platform:** Android (APK test / AAB Google Play)  
**Ürün adı:** **LP FAL**  
**Durum:** V1 kapsamı kilitli — yeni özellik ancak V1 tamamlandıktan sonra değerlendirilir.

---

## 1. Ürün hedefi

LP FAL; kahve falı ve tarot falını cihaz üzerinde çalışan yapay zekâ ile yorumlayan, fal sonucuna bağlı sohbet sunan, reklamla gelir üreten ve isteğe bağlı aylık reklamsız Premium abonelik içeren sade bir Android uygulaması olacaktır.

Ana hedefler:
- Hızlı, stabil ve kolay kullanılan bir fal uygulaması.
- Yapay zekâ yalnızca fal mekanizmasında kullanılacak.
- Kahve fotoğrafları ve fal konuşmaları mümkün olduğunca cihazdan çıkmayacak.
- Ana gelir modeli reklam olacak.
- Premium yalnızca basit bir aylık reklamsız kullanım modeli olacak.
- V1 gereksiz modüllerle büyütülmeyecek.

---

## 2. V1 kapsam kilidi

V1’de yalnızca aşağıdaki ana modüller bulunur:
1. Ana dashboard.
2. Kahve Falı.
3. Tarot Falı.
4. Fal sonucu ve fal sohbeti.
5. Geçmiş Fallar.
6. Premium / reklamsız aylık abonelik.
7. Profil / temel ayarlar.
8. Reklam sistemi.
9. Yerel Qwen AI motoru.

### V1’e eklenmeyecekler
- Burç / astroloji.
- Rüya tabiri.
- El falı.
- Numeroloji.
- Sosyal ağ / kullanıcılar arası mesajlaşma.
- Coin/kredi sistemi.
- Tek seferlik Premium satın alma.
- Çok katmanlı Premium paketler.
- Cloudflare AI / R2 / harici inference sunucusu.
- Genel amaçlı AI asistanı.

Bu kapsam V1 tamamlanana kadar değiştirilmez.

---

## 3. Marka ve görsel kimlik

### 3.1 İsim
Uygulamanın görünen adı her yerde **LP FAL** olacaktır.

### 3.2 Logo
Launcher, splash ve uygulama içi marka logosu MIZANGLOBAL reposunda bulunan mevcut Lefferion Prime/Mizan logosuyla aynı olacaktır:

`MIZANGLOBAL/assets/brand/lefferion-prime-logo.png`

Logo yeniden çizilmeyecek veya farklı bir sembolle değiştirilmeyecektir. Android adaptive icon gereksinimleri için aynı logodan türetilmiş foreground/background varyasyonları hazırlanabilir; marka geometrisi değiştirilmez.

### 3.3 UI yönü
- Ana tema açık ve sıcak olacaktır.
- Koyu mor/siyah “AI uygulaması” görünümü kullanılmayacaktır.
- Ana palet: kırık beyaz, krem, sıcak bej, kahve ve kontrollü bronz tonları.
- Ana yüzey örnekleri: `#FAF7F1`, `#EEE5D8`, `#5C4033`, `#B58B5A`, `#202020`.
- Okunabilirlik ve kontrast WCAG’a makul ölçüde uygun tutulacaktır.
- Gölgeler, gradientler ve dekoratif efektler ölçülü kullanılacaktır.

### 3.4 Uygulama içi görseller
- Kahve ve tarot görselleri yapay zekâ ile üretilmeyecektir.
- Lisansı ticari kullanıma uygun gerçek fotoğraf / kaliteli illüstrasyon / public-domain asset kullanılacaktır.
- Görseller uzaktan hotlink edilmeyecek; uygulama asset’i olarak JPEG/WebP/PNG biçiminde gömülecektir.
- Her üçüncü taraf görsel için kaynak ve lisans kaydı tutulacaktır: `assets/licenses/ASSET_LICENSES.md`.
- Dashboard’daki kart fonksiyonları görselin üzerine UI katmanı olarak bindirilecektir; görselin kendisi buton mantığı taşımayacaktır.

---

## 4. Dashboard — zorunlu görünüm

Ana ekran sade tutulacaktır.

### Üst alan
- Mizan/Lefferion Prime logosu.
- **LP FAL** adı.
- Sağ üstte profil/ayar erişimi.

### Ana kartlar
1. **Kahve Falı**
   - Gerçek ve kaliteli kahve/fincan fotoğrafı.
   - Kısa açıklama.
   - `Falına Bak` CTA.
2. **Tarot Falı**
   - Yüksek kaliteli tarot görseli.
   - Kısa açıklama.
   - `Tarot Açılımı` CTA.

### Devam alanı
Aktif/son fal varsa küçük `Son Falın / Falına Devam Et / Sohbete Dön` kartı gösterilir.

### Alt navigasyon
- Ana Sayfa
- Fallarım
- Premium
- Profil

Dashboard’da gereksiz carousel, haber, günlük burç, puan, coin veya içerik akışı bulunmayacaktır.

---

## 5. Kahve Falı

### 5.1 Fotoğraf girişi
- Kamera ve galeri desteklenecek.
- Kullanıcı 2–3 fincan fotoğrafı yükleyebilecek; 3 fotoğraf önerilecek.
- Fotoğraf yönü/EXIF gerektiğinde normalize edilecek.
- Analizden önce görüntü kalite kontrolü yapılacak:
  - bulanıklık,
  - aşırı karanlık/aydınlık,
  - fincan içinin görünürlüğü,
  - çok düşük çözünürlük,
  - yanlış görsel.
- Kalitesiz görselde AI çalıştırılmadan kullanıcıdan tekrar fotoğraf istenecek.

### 5.2 Görsel analiz pipeline’ı
Qwen’e tek komutla `fal bak` denmeyecektir.

Sıra:
1. Görüntüyü normalize et.
2. Fincanı ve kullanılabilir alanı tespit et.
3. Gerekirse ağız/orta/dip/kulp çevresi gibi crop’lar üret.
4. Qwen ile önce yalnızca yapılandırılmış görsel analiz üret.
5. Şekiller için konum + açıklama + güven skoru oluştur.
6. Düşük güvenli şekilleri kesin gerçek gibi kullanma.
7. Yapılandırılmış analizden ikinci aşamada fal metni üret.

Örnek ara veri:
```json
{
  "region": "upper_left",
  "shape": "bird_like",
  "confidence": 0.82
}
```

### 5.3 Halüsinasyon kontrolü
- Yüksek güvenli semboller ana yoruma alınır.
- Orta güvenli semboller “benziyor / çağrıştırıyor” diliyle kullanılabilir.
- Düşük güvenli semboller fal metnine sokulmaz.
- Model, görüntüde tespit etmediği sembolleri sırf metni süslemek için eklememelidir.

### 5.4 Fal sonucu
Sonuç ekranında en fazla şu başlıklar bulunur:
- Genel Yorum
- Aşk
- İş / Para
- Yol / Değişim

Sağlık teşhisi, ölüm zamanı, hamilelik kesinliği, hukuki sonuç veya garantili finansal kazanç yorumu üretilmez.

---

## 6. Tarot Falı

### 6.1 Deste
- V1’de tek bir kaliteli, tutarlı tarot destesi kullanılır.
- Tercih: yüksek çözünürlüklü ve lisans durumu açık Rider–Waite–Smith/public-domain kaynak.
- Kartların tümü aynı görsel standardında olmalıdır.
- Düşük çözünürlüklü, filigranlı veya karışık kaynaklı kartlar kullanılmayacaktır.

### 6.2 Açılım türleri
- Tek Kart
- Üç Kart: Geçmiş / Şimdi / Gelecek
- Beş Kart: detaylı açılım

### 6.3 AI kullanımı
Tarot kartı görselden tekrar tanınmayacaktır. Uygulama seçilen kartların ID/ad/pozisyon/düz-ters bilgisini doğrudan AI’a verir. Böylece kart tanıma hatası oluşmaz.

Qwen yalnızca fal yorumunu ve devam sohbetini üretir.

---

## 7. Fal Sohbeti

- Sohbet yalnızca kullanıcının aktif kahve/tarot falının bağlamında çalışır.
- Genel amaçlı chatbot değildir.
- Aktif falın yapılandırılmış analizi, sonucu ve sohbet geçmişi context olarak kullanılır.
- Kullanıcı kod, ödev, haber, genel bilgi vb. isterse AI nazikçe yalnız fal hakkında konuşabileceğini belirtir.
- Sohbet geçmişi cihazda tutulur.
- Yeni fal başlatıldığında yeni conversation context oluşturulur.
- Context boyutu sınırlı tutulur; eski mesajlar gerektiğinde yerel özetlenir.

---

## 8. Yerel AI — Qwen

### 8.1 Ana model
V1 ana hedefi **Qwen3.5-0.8B** ailesinin lisans açısından uygun resmi ağırlıklarından oluşturulmuş mobil/quantized sürümdür.

- Resmi model kaynağı kullanılacaktır.
- Uygulama için quantization yapılacaktır.
- Hedef ilk aday: Q4 sınıfı; gerçek cihaz benchmarkına göre daha uygun quantization seçilebilir.
- Model fal görevine prompt + gerektiğinde fine-tune ile özelleştirilecektir.
- Modelin Apache-2.0 ve ilgili üçüncü taraf lisans/NOTICE yükümlülükleri uygulama içinde `Açık Kaynak Lisansları` bölümünde korunacaktır.

### 8.2 Ağ politikası
AI motoru için:
- Cloudflare yok.
- Harici inference API yok.
- Kullanıcı fotoğrafı sunucuya yüklenmez.
- Fal promptu sunucuya gönderilmez.
- Fal sohbeti sunucuya gönderilmez.

AI inference cihazda yapılır.

### 8.3 Model dağıtımı
**DEBUG/TEST:**
- Model PC’den telefona/uygulama özel alanına yerel olarak aktarılabilir.
- Google Play yüklemesi zorunlu değildir.
- `tools/install_model.*` benzeri geliştirici aracıyla ADB/local test desteklenir.

**RELEASE:**
- Model ana APK içine gömülmez.
- Google Play’in on-demand asset/AI model dağıtım mekanizması kullanılacaktır.
- Model bir kez indirildikten sonra cihazda kalır.
- Uygulama modelin SHA-256 bütünlük kontrolünü yapar.
- Model güncellemesi uygulama/asset-pack sürümüyle yönetilir; bağımsız Cloudflare bağlantısı kurulmaz.

### 8.4 Cihaz hedefi
- 4 GB RAM: destek hedefi, agresif bellek optimizasyonu ve gerçek cihaz testi zorunlu.
- 6 GB RAM: ana hedef sınıf.
- 8 GB+ RAM: rahat hedef sınıf.
- Model yüklenirken OOM oluşmamalı; gerekirse görsel hazırlama ile text-generation evreleri ardışık çalıştırılır.
- Desteklenmeyen cihazlarda uygulama crash olmamalı; açık bir uyumluluk mesajı gösterilmelidir.

### 8.5 Performans hedefleri
Bunlar release-gate hedefidir ve gerçek cihazlarda ölçülür:
- UI cold start: orta sınıf cihazda yaklaşık ≤2.5 sn.
- Model warm-up: 6/8 GB sınıfta mümkün olduğunca ≤5–8 sn.
- Kahve fotoğraflarından ilk anlamlı sonuç: 6/8 GB sınıfta hedef ≤15 sn.
- Sohbet cevabı ilk token: hedef ≤3–5 sn warm durumda.
- 4 GB cihazlarda daha yüksek süre kabul edilebilir ancak UI donmamalıdır.

Ana thread hiçbir AI inference işiyle bloke edilmeyecektir.

---

## 9. Qwen doğruluk / kalite kabul kriterleri

Kahve falındaki en kritik konu görüntünün gerçekten okunmasıdır.

Release öncesi en az 100 gerçek fincan fotoğrafından oluşan sabit bir doğrulama seti hazırlanacaktır. Görsellerde insan tarafından işaretlenmiş belirgin bölgeler/semboller bulunacaktır.

Kontroller:
- Yüksek güvenli sembollerde hedef precision ≥ %80.
- Yüksek güvenli false-positive oranı hedef ≤ %15.
- Aynı fotoğraf tekrarında tamamen alakasız sembol sıçramaları kabul edilmez.
- Fal metni, yapılandırılmış analizde bulunmayan yüksek kesinlikli sembolleri uydurmamalıdır.
- Türkçe metin doğal, akıcı ve tekrar oranı düşük olmalıdır.
- En az 50 sabit prompt ile Türkçe kalite regresyon testi tutulur.

Bu hedefler sağlanmazsa önce pipeline/prompt/fine-tune düzeltilir; sırf özellik tamamlansın diye model onaylanmaz.

---

## 10. Reklam sistemi

Ana gelir modeli reklamlardır.

### 10.1 Ücretsiz kullanıcı
- Kahve falının tam sonucunu açmak için Rewarded Ad ana monetizasyon noktasıdır.
- Tarot yorumunu açmak için Rewarded Ad kullanılabilir.
- Fal sohbetinde belirli mesaj hakkından sonra Rewarded Ad ile yeni mesaj paketi açılır.
- Dashboard/geçmiş gibi uygun alanlarda ölçülü banner/native reklam kullanılabilir.
- Interstitial yalnız doğal geçiş noktalarında ve frequency-cap ile kullanılacaktır.

### 10.2 Reklam ilkeleri
- Kullanıcıyı yanlışlıkla reklama tıklatacak UI yapılmaz.
- Aynı işlemde art arda reklam spam’i yapılmaz.
- Fal sonucunu okurken ekranı beklenmedik interstitial ile bölmek yoktur.
- Reklam yüklenemezse uygulama sonsuz kilitlenmemelidir; kontrollü retry/fallback akışı bulunur.
- Reklam SDK’sı AI verisine erişmemelidir.

### 10.3 Premium
V1 Premium sadece **aylık reklamsız abonelik** olacaktır.

Premium:
- tüm reklamları kaldırır.

V1’de Premium için özel AI, VIP fal, coin veya ayrı içerik zorunlu değildir.

Satın alma Google Play Billing üzerinden yapılır ve `Satın alımları geri yükle` desteklenir.

---

## 11. Yerel veri ve gizlilik

Backend kullanıcı hesabı zorunlu değildir.

Cihazda tutulabilecekler:
- fal geçmişi,
- seçilen tarot kartları,
- fal metinleri,
- sohbet geçmişi,
- kullanıcı tercihleri,
- model durumu.

Kullanıcı:
- tek bir falı silebilmeli,
- tüm geçmişini silebilmeli,
- yerel uygulama verisini sıfırlayabilmelidir.

Fotoğraflar gereksiz yere kalıcı saklanmayacaktır. Kullanıcı isterse fal kaydına bağlanabilir; varsayılan davranış gizlilik ve depolama açısından minimum tutulur.

AdMob/Google Play Billing gibi SDK’ların veri kullanımı Google Play Data Safety ve gizlilik politikasında doğru açıklanacaktır.

---

## 12. Güvenlik ve içerik sınırları

- Uygulama açık biçimde eğlence/yorum amaçlı konumlandırılır.
- AI kesin gelecek garantisi vermez.
- Ölüm, ciddi hastalık, hamilelik, suç, hukuki hüküm veya garantili yatırım/şans kazancı gibi yüksek riskli kesin iddialar engellenir.
- Chatbot fal dışı genel asistan rolüne geçmez.
- Kullanıcı tarafından üretilen/AI tarafından üretilen uygunsuz içeriği bildirme mekanizması Google Play AI içerik politikalarına uygun biçimde sağlanır.
- Prompt injection ile fal sınırlarının aşılması test edilir.

---

## 13. Teknik uygulama ilkeleri

- Flutter tabanlı Android proje tercih edilir; Android native AI runtime gerektiğinde platform channel/FFI ile izole edilir.
- UI, AI, reklam, billing ve storage katmanları birbirinden ayrılır.
- Model dosyası Git reposuna normal source asset olarak commit edilmez.
- Büyük binary dosyaları kaynak kod geçmişini şişirmemelidir.
- Secrets/API anahtarları repoya hard-code edilmez.
- Release signing secret’ları GitHub’a plaintext eklenmez.
- AI inference background isolate/native worker üzerinde yürütülür.
- Crash-safe model loading ve low-memory handling zorunludur.

Önerilen modül ayrımı:
- `lib/features/home`
- `lib/features/coffee_fortune`
- `lib/features/tarot`
- `lib/features/fortune_chat`
- `lib/features/history`
- `lib/features/premium`
- `lib/core/ai`
- `lib/core/ads`
- `lib/core/billing`
- `lib/core/storage`
- `lib/core/theme`

---

## 14. Responsive / cihaz uyumluluğu

- Öncelik portrait Android telefon.
- 360dp küçük ekranlardan büyük telefon/tablet boyutlarına responsive olmalıdır.
- Notch, punch-hole, gesture navigation ve safe-area dikkate alınır.
- Tablet/BlueStacks üzerinde layout taşması olmayacaktır.
- Metin büyütme ile temel ekranlar kırılmamalıdır.
- Görseller `cover/contain` kararlarıyla kırpma hatası olmadan yönetilir.

---

## 15. Test zorunlulukları

### Fonksiyonel
- Fotoğraf seçme/kamera.
- Kalite kontrolü.
- Qwen model bulunamadı / model kurulum akışı.
- Kahve analizi.
- Fal üretimi.
- Tarot kart seçimi ve yorum.
- Fal chatbot context devamlılığı.
- Geçmiş kayıt/silme.
- Reklam akışları.
- Premium satın alma/restore.

### AI
- 100+ fincan görseli doğruluk seti.
- 50+ Türkçe sabit fal/chat promptu.
- Fal dışı istek reddi.
- Prompt injection testi.
- Offline AI testi.

### Performans
- 4 GB, 6 GB, 8 GB RAM sınıflarında test.
- Uzun sohbet sonrası bellek sızıntısı.
- Arka plan/foreground dönüşü.
- Model load/unload.
- 20+ ardışık fal çalıştırma stress testi.

### Ağ
Model bir kez cihaza geldikten sonra AI kullanımı sırasında ağ logları kontrol edilir. AI’ya ait fotoğraf/prompt/chat verisi dışarı çıkmamalıdır. Ağ yalnız reklam, Google Play/Billing ve kullanıcı tarafından açıkça başlatılmış gerekli mağaza işlemlerinde kullanılabilir.

---

## 16. Build / teslimat

Geliştirmede:
- debug APK,
- gerektiğinde universal test APK,
- lokal model kurulum akışı.

Finalde:
- imzalı release AAB,
- test amaçlı imzalı APK,
- Google Play için model asset dağıtımı,
- sürüm numarası/changelog,
- lisans dosyaları,
- gizlilik politikası ve mağaza Data Safety girdileri,
- temiz repo.

GitHub Actions en az:
- `flutter analyze`,
- testler,
- Android debug/release build doğrulaması
çalıştırmalıdır.

---

## 17. V1 final kabul kriterleri

LP FAL ancak aşağıdaki şartların tamamı sağlandığında final kabul edilir:

- [ ] Uygulama adı her yerde LP FAL.
- [ ] Mizan/Lefferion Prime mevcut logo asset’i doğru ve net kullanılıyor.
- [ ] Açık krem/sıcak dashboard onaylı tasarıma sadık.
- [ ] AI üretimi uygulama içi stok görsel yok.
- [ ] Kahve Falı uçtan uca çalışıyor.
- [ ] Fotoğraf kalite kontrolü çalışıyor.
- [ ] Qwen cihazda lokal çalışıyor.
- [ ] AI fotoğraf/prompt/chat verisini sunucuya göndermiyor.
- [ ] Kahve analizinde structured-analysis → fal üretimi iki aşamalı.
- [ ] Qwen doğruluk/regresyon test seti kabul hedeflerini karşılıyor.
- [ ] Tarot 1/3/5 kart akışı çalışıyor.
- [ ] Tarot görselleri yüksek kalite ve lisansı temiz.
- [ ] Fal sohbeti yalnız fal bağlamında çalışıyor.
- [ ] Geçmiş Fallar kayıt/silme çalışıyor.
- [ ] Rewarded/banner/interstitial frequency-cap doğru çalışıyor.
- [ ] Premium aylık reklamsız kullanım ve restore çalışıyor.
- [ ] Reklam/Premium yokluğunda crash veya dead-end yok.
- [ ] 4/6/8 GB cihaz sınıflarında temel QA tamam.
- [ ] Tablet/BlueStacks responsive kontrol tamam.
- [ ] Offline AI testi geçti.
- [ ] Gizlilik, lisans ve Google Play AI içerik gereksinimleri tamam.
- [ ] Debug APK ve release AAB başarıyla derleniyor.
- [ ] Release build temiz cihazda kurulum/açılış testini geçiyor.

**Bu checklist’in herhangi bir maddesi eksikken V1 “final” olarak işaretlenmez.**
