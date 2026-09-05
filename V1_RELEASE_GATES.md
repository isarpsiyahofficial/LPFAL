# LP FAL — V1 Eksik Kontrolü ve Ek Release Gates

**Durum:** ZORUNLU / normatif  
**Bağlı dosyalar:** `SPECIFICATION.md`, `TODO.md`  
**Amaç:** İlk şartname ve TODO denetiminde bulunan belirsizlikleri kapatmak. Bu dosyadaki maddeler V1 final checklist'inin parçasıdır. `SPECIFICATION.md` veya `TODO.md` ile çelişirse daha güvenli/daha açık olan bu gate uygulanır.

---

## 1. Ürün kapsamı ve dil kilidi

- V1 kullanıcı arayüzü ve fal üretim dili **Türkçe** olacaktır.
- Kod i18n'e hazır tutulabilir ancak V1'e ikinci dil eklenmez.
- V1'e push notification, burç, rüya, el falı, numeroloji, coin/kredi, sosyal özellik, genel AI asistanı veya yeni fal türü eklenmez.
- Hedef kitle Google Play Console'da açıkça beyan edilir. Çocuklara yönelik tasarım yapılmaz; V1 için varsayılan mağaza hedefi yetişkin kullanıcıdır. Hedef kitle değiştirilirse Families/çocuk reklam politikaları release öncesi yeniden denetlenir.

---

## 2. Android kimliği ve platform temeli

- Android `applicationId` V1 başlamadan sabitlenir. Önerilen kalıcı kimlik: `com.lefferionprime.lpfal`.
- `minSdk`, `compileSdk` ve `targetSdk` kullanılan AI runtime ile uyumlu seçilir; release anındaki Google Play zorunlu target API seviyesi karşılanır.
- Signing key/keystore repoya konmaz; güvenli yedeği ayrı tutulur.
- Android Auto Backup / cloud backup kuralları açıkça tanımlanır. Fal geçmişi, sohbet, ham/geçici fotoğraflar, model dosyaları ve AI cache'leri izinsiz cloud backup'a gitmez.

---

## 3. Fotoğraf izinleri ve geçici dosya politikası

- Galeri seçimi için mümkün olan Android sürümlerinde **System Photo Picker** tercih edilir; geniş galeri/storage izni istenmez.
- Kamera izni yalnız kullanıcı kamera akışını başlattığında istenir.
- Seçilen fotoğraflar yalnız app-private geçici alanda işlenir.
- Ham fincan fotoğrafları varsayılan olarak fal üretildikten sonra silinir.
- Production loglarında fotoğraf path'i, prompt, chat metni veya structured-analysis içeriği yazılmaz.
- Kullanıcı verileri gelecekte model fine-tune/eğitimi için **varsayılan olarak kullanılmaz**; böyle bir özellik ancak ayrı açık opt-in ile yapılabilir.

---

## 4. Çoklu fincan fotoğrafı birleştirme

2–3 fotoğraf birbirinden bağımsız üç fal gibi yorumlanmayacaktır.

Zorunlu pipeline:
1. Her fotoğrafta kullanılabilir fincan bölgelerini çıkar.
2. Aynı fincanın farklı açılarından gelen sembolleri konum/benzerlik açısından eşleştir.
3. Aynı sembolü birden fazla fotoğrafta gördüğü için iki/üç kez sayma.
4. Fotoğraflar arasında çelişen bulguları `uncertain` olarak işaretle veya ele.
5. Final structured-analysis tek bir **merged cup analysis** üretir.
6. Fal metni yalnız merged analysis üzerinden oluşturulur.

Release test seti tek fotoğraf örneklerinin yanında gerçek 2–3 fotoğraflı fincan grupları da içermelidir.

---

## 5. Qwen confidence kalibrasyonu — kritik

Qwen'in metin olarak ürettiği `confidence: 0.82` değeri **tek başına istatistiksel olasılık kabul edilmeyecektir**.

- Self-reported confidence yalnız ham sinyal olarak kullanılır.
- Gerçek threshold'lar 100+ fincan doğrulama seti üzerinde kalibre edilir.
- Cross-view agreement, crop tekrarları ve false-positive ölçümü threshold kararına dahil edilir.
- High/medium/low sınıfları validation sonuçlarıyla sabitlenir.
- Model/prompt/generation parametreleri değiştiğinde kalibrasyon testi yeniden çalıştırılır.
- Test setinde `hiç belirgin sembol yok`, zor ışık, bulanıklık, yanıltıcı desen ve negatif örnekler bulunmalıdır.

**Release gate:** Görsel modelin kendi confidence sayısını kör biçimde kullanmak yasaktır.

---

## 6. AI sürümleme ve yeniden üretilebilirlik

Her release aşağıdakileri kayıt altına alır:
- kaynak Qwen model/revision,
- quantization türü,
- model SHA-256,
- vision projector/encoder dosyası ve SHA-256 (ayrıysa),
- system prompt sürümü,
- coffee-analysis prompt sürümü,
- tarot prompt sürümü,
- chat prompt sürümü,
- temperature/top-p/top-k/max token gibi generation ayarları,
- kullanılan native inference runtime sürümü.

QA regresyonu bu sürüm kimliğiyle ilişkilendirilir. Model veya prompt değişikliği AI regresyon testini tekrar tetikler.

---

## 7. Model indirme / cihaz uygunluğu

- Uygulama model indirmeden önce cihaz RAM/ABI/runtime uyumluluğunu kontrol eder.
- Gerekli boş depolama alanı kontrol edilir; indirme boyutu kullanıcıya açıkça gösterilir.
- Model indirme kullanıcı tarafından başlatılabilir, iptal/retry destekler ve UI'ı kilitlemez.
- Eksik/yarım/corrupt model hiçbir zaman inference'a verilmez.
- Model update sonrası eski sürüm güvenli biçimde temizlenir.
- AI işi iptal edilebilir; kullanıcı ekranı terk ettiğinde veya sistem low-memory durumuna girdiğinde runaway inference oluşmaz.
- Thermal throttling/aşırı ısınma senaryosu gözlemlenir; uzun inference'ta UI donmamalıdır.

### Play dağıtım notu
`Play for On-device AI` 2026 itibarıyla beta durumunda olduğundan production release sırasında resmi kullanılabilirlik yeniden doğrulanacaktır. Kullanılan Play dağıtım yöntemi değişse bile:
- model base APK içine gömülmez,
- harici Cloudflare/CDN zorunluluğu oluşturulmaz,
- debug/local model yolu korunur,
- model bir kez geldikten sonra inference cihazda kalır.

---

## 8. Ücretsiz / offline kullanım politikası

Yerel AI offline çalışabilir; fakat monetizasyon gate'i ayrı ele alınır.

### Ücretsiz kullanıcı
- Daha önce reklamlarla açılmış fal sonuçları offline okunabilir.
- Yeni kahve falının tam sonucunu açmak için **2 başarılı Rewarded Ad** gerekir.
- Yeni tarot yorumunu açmak için **2 başarılı Rewarded Ad** gerekir.
- Gated chat mesaj paketini açmak için **2 başarılı Rewarded Ad** gerekir.
- İlk reklamın tamamlanması tek başına hiçbir içerik/mesaj hakkı açmaz.
- İkinci reklam da başarılı `reward earned` callback'i üretmeden reward entitlement verilmez.
- İnternet/reklam yoksa uygulama crash/freeze olmaz; kullanıcı `Tekrar Dene` veya geri çıkış alır.
- **Reklam başarısız oldu diye ücretsiz entitlement otomatik verilmez.** Bu, airplane-mode ile reklam bypass'ını engeller.

### Premium kullanıcı
- Model kurulmuşsa kahve, tarot, geçmiş ve fal sohbeti reklamsız/offline kullanılabilir.
- Premium durumu Play Billing ile çevrimiçiyken periyodik doğrulanır; yalnız local boolean kalıcı kaynak kabul edilmez.

---

## 9. Reklam ekonomisi — V1 varsayılanı

V1'de monetizasyon davranışı belirsiz bırakılmayacaktır.

### 9.1 İki reklam = bir reward kuralı
- **Kahve Falı:** her yeni tam fal sonucunu açmak için **2 Rewarded Ad**.
- **Tarot:** her yeni açılımın tam yorumunu açmak için **2 Rewarded Ad**.
- **Fal Sohbeti:** fal başına ilk takip sorusu ücretsiz; ardından her 3 kullanıcı mesajlık paketi açmak için **2 Rewarded Ad**.
- Kullanıcıya reward başlamadan önce açıkça `Bu içeriği açmak için 2 reklam izle` bilgisi gösterilir.
- UI ilerlemesi açık biçimde `0/2 → 1/2 → 2/2` gösterilir.
- Her rewarded reklam **ayrı ayrı kullanıcı tarafından olumlu biçimde başlatılır**; ilk reklam bitti diye ikinci reklam otomatik açılmaz.
- İlk reklam tamamlandığında `1/2 tamamlandı` state'i yazılır ancak reward verilmez.
- İkinci reklamın başarılı `reward earned` callback'i geldikten sonra tek reward entitlement açılır.
- İlk reklam tamamlandıktan sonra ikinci reklam geçici olarak yüklenemezse kullanıcı ilk reklamı anında tekrar izlemek zorunda bırakılmaz; aynı reward transaction içindeki `1/2` ilerlemesi güvenli biçimde korunur ve kullanıcı ikinci reklamı daha sonra tekrar deneyebilir.
- Reward transaction başka fal/açılım/chat paketine aktarılamaz; her gate kendi `rewardTransactionId` ile izlenir.
- Tamamlanmamış reward transaction sonsuza kadar tutulmaz; uygulama tarafından belirlenen makul bir süre/akış sonunda expire edilir.

### 9.2 Genel reklam kuralları
- Rewarded reklam kullanıcı tarafından açıkça başlatılır; otomatik açılmaz.
- Ödül yalnız ikinci reklam dahil gerekli tüm SDK `reward earned` callback'leri tamamlandıktan sonra verilir.
- Rewarded tamamlanmadan entitlement yazılmaz.
- Interstitial, rewarded gösteriminden hemen önce/sonra gösterilmez ve sonuç okuma/chat akışını bölmez.
- Interstitial için V1 varsayılan frequency-cap: kullanıcı başına en fazla 1 gösterim / 10 dakika ve yalnız doğal ekran geçişinde.
- Banner/native yalnız dashboard/geçmiş gibi uygun yüzeylerde; AI sonuç metninin içine karışmaz.
- Premium entitlement aktifse ad request dahi mümkün olduğunca oluşturulmaz.

Bu değerler kodda tek bir `MonetizationConfig` altında tutulur; UI içine dağınık magic number olarak yazılmaz. V1 sabiti: `rewardedAdsPerUnlock = 2`.

---

## 10. Premium / Play Billing lifecycle

Premium tek ürün olmaya devam eder: **aylık, otomatik yenilenen, reklamsız kullanım**.

Satın alma ekranı satın almadan önce açıkça göstermelidir:
- yerel para birimindeki fiyat,
- aylık dönem,
- otomatik yenileme,
- nasıl iptal/yönetileceği,
- Premium'un yalnız reklamları kaldırdığı.

Zorunlu Billing durumları:
- successful purchase,
- pending purchase,
- canceled purchase flow,
- restore/query existing purchase,
- renewal,
- grace period,
- account hold,
- expired/canceled subscription,
- app process death sırasında yarım kalan purchase callback recovery.

Grace period'da Play'in aktif kabul ettiği entitlement korunur; account hold/expiry durumunda reklamlı moda dönüş test edilir. `Aboneliği Yönet` bağlantısı kullanıcıyı Google Play subscription management ekranına götürür.

Backend olmadan mutlak anti-tamper garanti edilmez; V1'de source of truth çevrimiçiyken Google Play Billing state'idir, cache yalnız offline UX içindir.

---

## 11. AI içerik raporlama — ağ politikasındaki tek açık istisna

Google Play generative-AI uygulamalarında uygulama içi report/flag mekanizması gerektirdiği için yalnız kullanıcı tarafından başlatılan **AI içerik raporu** minimal bir HTTPS endpoint'e gönderilebilir.

Kurallar:
- Bu endpoint AI inference yapmaz.
- Otomatik telemetry değildir.
- Kullanıcı rapor butonuna basmadan veri gönderilmez.
- Varsayılan payload: report reason, ilgili AI metninin seçilen bölümü, app/model/prompt version, zaman damgası.
- Ham fincan fotoğrafı varsayılan olarak gönderilmez.
- Kullanıcı açıklama ekleyebilir.
- Report endpoint sağlayıcısı release öncesi sabitlenir ve Privacy Policy'de açıklanır.

Bunun dışında fotoğraf/prompt/chat AI amacıyla sunucuya çıkmaz.

---

## 12. Güvenlik / içerik filtresi genişletmesi

Mevcut yüksek-risk filtrelerine ek olarak:
- kendine zarar verme teşviki,
- şiddet/suç yönlendirmesi,
- çocuklara yönelik uygunsuz içerik,
- nefret/taciz,
- cinsel içerik üretme talebi,
- kullanıcıyı korkutmak amacıyla kesin felaket/ölüm iddiası
engellenir veya güvenli, fal bağlamında nötr yanıta dönüştürülür.

Chat scope kontrolü yalnız prompt ile bırakılmaz; uygulama katmanında da kategori/sistem kuralı bulunur.

---

## 13. Tarot draw doğruluğu

- Aynı açılım içinde aynı kart iki kez çekilmez.
- Kart seçimi biased olmamalıdır; `Random.secure()` veya eşdeğer güvenilir RNG kullanılabilir.
- Düz/ters state seçimi kart draw'dan ayrı ve açık bir kuralla üretilir.
- 78 kart metadata + görsel mapping için otomatik bütünlük testi bulunur.
- Eksik/yanlış kart asset'i release'i bloklar.

---

## 14. Erişilebilirlik / UI kalite gate'i

- Dokunma hedefleri Android önerisine uygun, mümkün olduğunca en az 48dp.
- TalkBack için anlamlı semantics/labels.
- Font scaling'de kritik CTA ve sonuç metinleri taşmaz.
- Kontrast yalnız dekoratif değil, gerçek ekran görüntüsü üzerinde kontrol edilir.
- Loading/AI state'leri yalnız animasyonla değil metin/semantic state ile de anlaşılır.

---

## 15. Analytics / telemetry politikası

V1'e ürün analytics veya üçüncü taraf davranış izleme SDK'sı varsayılan olarak eklenmez.

İzin verilen ağ bileşenleri:
- AdMob + UMP,
- Google Play Billing / Play delivery,
- kullanıcı tarafından açıkça başlatılmış AI report endpoint'i.

Yeni analytics/crash SDK eklenmesi ayrı karar ve Privacy/Data Safety güncellemesi gerektirir. Eğer crash reporting eklenirse prompt/chat/fotoğraf ve structured analysis scrub edilmeden gönderilemez.

---

## 16. AdMob / mağaza release ayrıntıları

- Development'ta yalnız AdMob test ad unit ID'leri kullanılır.
- Production ID geçişi release checklist'te doğrulanır.
- UMP consent akışı gerekli bölgelerde reklam request'inden önce tamamlanır.
- `app-ads.txt` uygulanabilirliği/domain yayını release öncesi kontrol edilir.
- Store listing, uygulamanın AI fal ürettiğini ve Premium'un reklamsız abonelik olduğunu yanıltıcı olmayacak şekilde açıklar.
- Privacy Policy için herkese açık kalıcı HTTPS URL bulunur.
- Support/Privacy/Open Source Licenses/Subscription Management erişimi Profil/Ayarlar içinde bulunur.

---

## 17. Ek QA matrisi

Mevcut testlere ek olarak:
- [ ] 2–3 fotoğraflı aynı fincan cross-view fusion testi.
- [ ] Sembol bulunmayan negatif fincan testleri.
- [ ] Confidence calibration testi.
- [ ] Photo Picker izin testi; broad storage permission olmadığını doğrula.
- [ ] Geçici fotoğraf cleanup testi.
- [ ] Android Auto Backup exclusion testi.
- [ ] Model download öncesi disk-space testi.
- [ ] Download cancel/resume/retry testi.
- [ ] Inference cancel/background/low-memory testi.
- [ ] Thermal/uzun inference testi.
- [ ] Free offline ad-bypass testi.
- [ ] İlk rewarded tamamlandığında reward verilmediği testi.
- [ ] `0/2 → 1/2 → 2/2` progress/state testi.
- [ ] Her iki rewarded reklamın ayrı kullanıcı opt-in'i gerektirdiği testi.
- [ ] İkinci rewarded callback olmadan entitlement verilmediği testi.
- [ ] İlk reklam sonrası ikinci reklam load-fail/retry ve progress korunumu testi.
- [ ] Reward transaction'ın başka fala/açılıma taşınamadığı testi.
- [ ] Interstitial frequency-cap testi.
- [ ] Pending billing testi.
- [ ] Grace period testi.
- [ ] Account hold/expiry testi.
- [ ] Subscription management link testi.
- [ ] AI report gönderimi ve privacy payload testi.
- [ ] Production build'de hassas debug logging kapalı testi.
- [ ] 78 tarot card metadata/asset integrity testi.
- [ ] TalkBack + font scaling smoke testi.

---

# EK BLOKLAYICI OLMAZSA OLMAZLAR

Aşağıdakilerden biri eksikse V1 final değildir:

1. Qwen self-reported confidence kalibre edilmeden gerçek olasılık olarak kullanılmıyor.
2. 2–3 fincan fotoğrafı tek merged analysis'e birleştiriliyor; duplicate semboller sayılmıyor.
3. Galeri için broad storage izni yerine Photo Picker/scoped yaklaşım kullanılıyor.
4. Ham fincan fotoğrafları varsayılan olarak inference sonrası temizleniyor.
5. Kullanıcı fal/chat/fotoğrafları varsayılan olarak eğitim verisine dönüşmüyor.
6. Free offline kullanım Rewarded Ad gate'ini bypass edemiyor.
7. **Her ücretsiz reward unlock için 2 Rewarded Ad gerekiyor; tek reklam reward vermiyor.**
8. İki reklam da ayrı ayrı kullanıcı tarafından başlatılıyor ve UI `0/2 → 1/2 → 2/2` ilerlemesini açık gösteriyor.
9. Reward yalnız ikinci reklam dahil gerekli iki başarılı `reward earned` callback'inden sonra veriliyor.
10. Premium Billing pending/grace/account-hold/expiry senaryoları test edilmiş.
11. Model/prompt/runtime sürümü ve SHA'ları release ile kayıtlı.
12. Model download öncesi cihaz ve disk uygunluğu kontrol ediliyor.
13. Play model dağıtım yöntemi production release tarihinde yeniden doğrulanmış.
14. Kullanıcı tarafından başlatılan in-app AI report akışı çalışıyor.
15. Android backup ile hassas yerel veriler izinsiz buluta gitmiyor.
16. Production loglarında fotoğraf/prompt/chat sızıntısı yok.
17. Tarot 78 kart mapping integrity testi geçiyor.
18. TalkBack/font scaling temel erişilebilirlik testi geçiyor.
19. V1 Türkçe kapsamı korunuyor; plansız çok-dil veya yeni özellik eklenmiyor.

**Bu dosya `TODO.md` Faz 18 final kontrolünün zorunlu girdisidir.**