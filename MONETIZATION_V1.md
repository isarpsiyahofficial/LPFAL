# LP FAL — V1 Monetizasyon Politikası

**Durum:** ZORUNLU / normatif  
**Öncelik:** Reklam zamanlaması ve yerleşimi konusunda bu dosya ana kaynaktır.  
**Billing / restore normatif eki:** `BILLING_RESTORE_SPEC.md`

## 1. Premium kullanıcı

V1 Premium yalnız **aylık otomatik yenilenen Google Play aboneliğidir**. Lifetime/non-consumable Premium V1'de yoktur.

Premium aktifse reklam sistemi tamamen kapalıdır:
- Rewarded yok.
- Süreye dayalı interstitial yok.
- App Open yok.
- Banner/native yok.
- Reklam request'i mümkün olduğunca oluşturulmaz.
- Yüklenmiş reklam nesneleri merkezi ad service tarafından dispose edilir.
- Kahve, Tarot, El Falı ve fal sohbeti reklam beklemeden kullanılır.

Premium entitlement'ın online source of truth'u Google Play'dir; yerel Premium cache'i tek başına ücretli Premium üretmez.

## 2. Google Play purchase / restore mimarisi

LP FAL, MIZANGLOBAL'da kullanılan başarılı mimari deseni LP FAL'in aylık abonelik modeline uyarlayacaktır. MIZAN reposuna hiçbir değişiklik yapılmaz ve MIZAN'ın lifetime ürün mantığı kopyalanmaz.

Zorunlu davranış:
- `purchaseStream` merkezi PurchaseService tarafından dinlenir.
- Satın alma ve restore aynı entitlement validation hattından geçer.
- Uygulama açılışı, app resume ve internet reconnect sırasında sessiz owned-purchase/subscription sync yapılır.
- Clean install / reinstall / app-data-clear sonrası aynı Google Play hesabındaki aktif abonelik restore edilebilir.
- Premium/Ayarlar ekranında ek recovery yolu olarak `Satın Alımları Geri Yükle` bulunur.
- Local `isPremium` boolean source of truth değildir.
- Play purchase token / verification data boş veya geçersizse ücretli Premium verilmez.
- Duplicate purchase callbacks idempotent işlenir.
- Initial purchase `pending` iken Premium açılmaz.
- Purchase cancel/error ücretsiz Premium üretmez.
- Process death sonrası bir sonraki launch/resume sync doğru entitlement'ı geri kurar.
- Fiyat/para birimi hard-code edilmez; Play `ProductDetails` kullanılır.
- `Aboneliği Yönet` Google Play subscription management akışını açar.

Abonelik state davranışı:
- active/renewed → Premium aktif,
- grace period → Premium aktif ve sıfır reklam,
- kullanıcı iptal etmiş fakat paid-through-end devam ediyor → bitişe kadar Premium aktif,
- account hold → Premium kapalı,
- expired → Premium kapalı,
- revoked/refunded ve entitlement artık yok → Premium kapalı.

Detaylı source-of-truth, offline cache, process-death ve QA kuralları `BILLING_RESTORE_SPEC.md` içindedir.

## 3. Rewarded unlock

Ücretsiz kullanıcı:
- Kahve falı tam sonucu: **2 Rewarded Ad**.
- Tarot tam yorumu: **2 Rewarded Ad**.
- El Falı tam yorumu: **2 Rewarded Ad**.
- Fal sohbetinde yeni mesaj paketi: **2 Rewarded Ad**.

Kurallar:
- `0/2 → 1/2 → 2/2` ilerlemesi gösterilir.
- İki reklam ayrı ayrı kullanıcı tarafından başlatılır.
- İkinci reklam otomatik açılmaz.
- Tek reklam reward vermez.
- Reward yalnız iki başarılı `reward earned` callback'inden sonra açılır.
- İlk reklamdan sonra ikinci reklam geçici olarak yüklenemezse aynı transaction içindeki `1/2` state'i güvenli biçimde korunur.
- Reward transaction başka fal/açılım/chat paketine taşınamaz.
- Reklam failure ücretsiz entitlement üretmez.

## 4. Süreye dayalı interstitial — 90 saniye

V1 sabiti:
`timedInterstitialEligibilitySeconds = 90`

90 saniye zorla reklam açma süresi değildir; yalnız yeni interstitial için uygunluk üretir.

- Yalnız foreground + aktif kullanım süresi sayılır.
- Background, reklam izleme ve model indirme süresi sayılmaz.
- 90 saniye dolunca `timedInterstitialEligible = true` olur.
- Reklam yalnız sonraki doğal/güvenli ekran geçişinde gösterilir.
- Gösterim sonrası sayaç sıfırlanır ve yeni 90 saniye dönemi başlar.
- Premium kullanıcıda timer çalışmaz.

Timed interstitial şu anlarda gösterilmez:
- kahve fotoğrafı seçme/çekme,
- el fotoğrafı seçme/çekme,
- Qwen analiz/inference,
- fal sonucunu aktif okuma,
- tarot kart seçme/çevirme,
- el görüntü kalite/analiz akışı,
- chat mesaj yazma veya AI cevap üretme,
- rewarded transaction,
- rewarded reklamın hemen öncesi/sonrası,
- billing/restore/consent/permission akışları.

## 5. Banner reklamlar

### Telefon / portrait
- Ana format: **anchored adaptive banner**.
- Banner yalnız güvenli ekranlarda üst veya alt bölgede gösterilir.
- Tercih edilen yüzeyler: Dashboard, Fallarım/Geçmiş, Profil/Ayarlar.
- Aynı telefonda eşzamanlı hem üst hem alt banner gösterilmez.
- Alt banner bottom navigation'dan; üst banner app bar/CTA'dan açık biçimde ayrılır.

### Tablet / BlueStacks / geniş ekran
- İçeriği daraltmadan sağ veya sol tek reklam kolonu kullanılabilir.
- Reklam kolonu ana içerikten görsel olarak ayrılır.
- İki yan reklam V1 varsayılanı değildir.

### Banner gösterilmeyecek ekranlar
- Kahve fotoğrafı çekme/seçme.
- El fotoğrafı çekme/seçme.
- Qwen analiz/loading.
- Fal chatbot ekranı.
- Tarot kart seçme/çevirme.
- El analiz/sonuç üretim state'i.
- Rewarded akışı.
- Premium/billing/restore checkout.
- Permission/consent modal akışları.

### Banner kalite kuralları
- CTA, bottom navigation, chat input, galeriler ve diğer etkileşimli öğelerden güvenli mesafede.
- Banner hiçbir butonun parçası gibi görünmez.
- Fal metninin içine veya sohbet mesajları arasına yerleştirilmez.
- Premium aktifse banner container tamamen kaldırılır.

## 6. App Open

- Timed interstitial'dan ayrı mekanizma.
- İlk açılışlarda agresif kullanılmaz.
- Rewarded/timed interstitial ile çakışmaz.
- Kullanıcı kritik capture/analysis/billing/restore akışına dönüyorsa gösterilmez.
- Premium'da kapalıdır.

## 7. Consent / AdMob servis prensibi

- UMP/consent çözülmeden reklam request'i yapılmaz.
- Premium suppress edilmişse Mobile Ads gereksiz yere initialize edilmez.
- Premium sonradan aktif olursa yüklü reklamlar dispose edilir.
- Premium entitlement kaybolursa reklamlar billing ekranının ortasında aniden gösterilmez; consent kontrolünden sonra güvenli UI noktasında yeniden hazırlanır.
- Aynı anda birden fazla full-screen reklam gösterilmez.
- Reward yalnız gerçek `onUserEarnedReward` callback'i ile sayılır.
- Dismiss/failure sonrası ad objesi dispose edilir ve Free kullanıcı için kontrollü preload yapılabilir.

## 8. Merkezi config

```text
rewardedAdsPerUnlock = 2
timedInterstitialEligibilitySeconds = 90
premiumProduct = monthlyAutoRenewingSubscription
```

Banner placement, App Open, interstitial gating, Google Play product ID/base-plan referansı ve AdMob ID seçimi merkezi `MonetizationConfig/AdPolicy` üzerinden yönetilir; ekranlara magic number/string dağılmaz.

Test/release ayrımı:
- Debug/test build yalnız Google test AdMob ID'lerini kullanır.
- Production ID'ler build environment/config üzerinden gelir.
- Release build'de boş/bozuk production ID veya test ID sızıntısı release gate fail eder.

## 9. Release QA

### Reklam
- [ ] Kahve/Tarot/El/chat unlock için 2 Rewarded gerekiyor.
- [ ] Tek rewarded unlock vermiyor.
- [ ] 90 saniyeden önce timed interstitial uygunluğu oluşmuyor.
- [ ] 90 saniye dolunca reklam anında zorla açılmıyor.
- [ ] Timed interstitial yalnız doğal geçişte gösteriliyor.
- [ ] Kahve/El capture veya AI inference sırasında timed ad yok.
- [ ] Rewarded transaction ile timed interstitial çakışmıyor.
- [ ] Phone banner interaktif öğelerden güvenli mesafede.
- [ ] Tablet/BlueStacks side banner ana içeriği bozmuyor.
- [ ] Chat/analysis/capture ekranlarında banner yok.
- [ ] Premium aktifken hiçbir reklam türü, request'i veya boş reklam container'ı görünmüyor.
- [ ] Premium aktifleşince yüklenmiş reklam objeleri dispose ediliyor.
- [ ] Test build'lerinde yalnız AdMob test ID'leri kullanılıyor.
- [ ] Release build'lerinde yalnız doğrulanmış production ID'leri kullanılıyor.

### Google Play Billing / Restore
- [ ] Aylık abonelik purchase başarılı.
- [ ] Pending ödeme Premium açmıyor.
- [ ] Cancel/error Premium açmıyor.
- [ ] Clean install + aynı Play hesabı aktif abonelik otomatik restore.
- [ ] Reinstall/app-data-clear sonrası restore.
- [ ] Manuel `Satın Alımları Geri Yükle` çalışıyor.
- [ ] App resume/reconnect silent sync çalışıyor.
- [ ] Process death recovery çalışıyor.
- [ ] Renewal Premium'u kesmiyor.
- [ ] Grace period Premium/reklamsız kalıyor.
- [ ] Cancelled paid-through-end bitişe kadar Premium.
- [ ] Account hold/expiry/revoked entitlement yoksa Premium kapanıyor.
- [ ] Duplicate callbacks idempotent.
- [ ] Store fiyatı hard-code değil Play metadata'sından geliyor.

**Bu dosya ve `BILLING_RESTORE_SPEC.md` V1 final kontrolünün zorunlu girdileridir.**