# LP FAL — V1 Monetizasyon Politikası

**Durum:** ZORUNLU / normatif  
**Öncelik:** Reklam zamanlaması/yerleşimi konusunda bu dosya ana kaynaktır. Premium ürün tipi ve isim standardında `PRODUCT_MODEL_V1.md` ana kaynaktır.

## 1. Premium kullanıcı

LP FAL V1'de Premium **abonelik değildir**. Tek seferlik Google Play satın alımıyla açılır.

Kullanıcıya görünen ürün adı:
- **LP FAL Premium**

Satın alma açıklaması:
- **Tek seferlik satın alım · Abonelik değildir.**

Premium aktifse reklam sistemi tamamen kapalıdır:
- Rewarded yok.
- Süreye dayalı interstitial yok.
- App Open yok.
- Banner/native yok.
- Reklam request'i mümkün olduğunca oluşturulmaz.
- Yüklenmiş reklam nesneleri dispose edilir.
- Boş reklam container'ı bırakılmaz.
- Kahve, Tarot, Rüya Tabiri, El Falı ve bunlara bağlı sohbetler reklam beklemeden kullanılır.

## 2. Rewarded unlock — Free kullanıcı

Ücretsiz kullanıcı:
- Kahve falı tam sonucu: **2 Rewarded Ad**.
- Tarot tam yorumu: **2 Rewarded Ad**.
- Rüya Tabiri tam yorumu: **2 Rewarded Ad**.
- El Falı tam yorumu: **2 Rewarded Ad**.
- Fal/rüya sohbetinde yeni mesaj paketi: **2 Rewarded Ad**.

Kurallar:
- `0/2 → 1/2 → 2/2` ilerlemesi gösterilir.
- İki reklam ayrı ayrı kullanıcı tarafından başlatılır.
- İkinci reklam otomatik açılmaz.
- Tek reklam reward vermez.
- Reward yalnız iki başarılı `reward earned` callback'inden sonra açılır.
- İlk reklamdan sonra ikinci reklam geçici olarak yüklenemezse aynı transaction içindeki `1/2` state'i güvenli biçimde korunur.
- Reward transaction başka fal/rüya/açılım/chat paketine taşınamaz.
- Reklam failure ücretsiz entitlement üretmez.

## 3. Süreye dayalı interstitial — 90 saniye

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
- rüya metni yazma/düzenleme/gönderme,
- Qwen analiz/inference,
- fal/rüya sonucunu aktif okuma,
- tarot kart seçme/çevirme,
- el görüntü kalite/analiz akışı,
- chat mesaj yazma veya AI cevap üretme,
- rewarded transaction,
- rewarded reklamın hemen öncesi/sonrası,
- billing/consent/permission akışları.

## 4. Banner reklamlar

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
- Rüya giriş/yazma ekranı.
- Qwen analiz/loading.
- Fal/Rüya chatbot ekranı.
- Fal/Rüya sonuç ekranı.
- Tarot kart seçme/çevirme.
- El analiz/sonuç üretim state'i.
- Rewarded akışı.
- Premium/billing checkout.
- Permission/consent modal akışları.

### Banner kalite kuralları
- CTA, bottom navigation, chat input, galeriler ve diğer etkileşimli öğelerden güvenli mesafede.
- Banner hiçbir butonun parçası gibi görünmez.
- Fal/rüya metninin içine veya sohbet mesajları arasına yerleştirilmez.
- Premium aktifse banner container tamamen kaldırılır.
- Kullanıcının fal/rüya içeriğinden hassas reklam profili türetilmez.

## 5. App Open

- Timed interstitial'dan ayrı mekanizma.
- İlk açılışlarda agresif kullanılmaz.
- Rewarded/timed interstitial ile çakışmaz.
- Kullanıcı kritik capture/dream-entry/analysis/billing akışına dönüyorsa gösterilmez.
- Premium'da kapalıdır.

## 6. Google Play satın alma / restore

Tek ücretli ürün kalıcı Premium entitlement veren tek seferlik Google Play ürünüdür.

- Purchase stream merkezi dinlenir.
- Uygulama açılış/resume/internet geri gelişi sırasında owned purchases sessizce senkronize edilir.
- Aynı Google Play hesabında reinstall/yeni cihaz restore edilmelidir.
- Kullanıcı için ayrıca `Satın Alımı Geri Yükle` aksiyonu bulunur.
- Purchase ve restore aynı doğrulama hattından geçer.
- Yerel cache tek başına Premium vermez.
- Premium entitlement başarılı Play kanıtına dayanır.
- Duplicate callback idempotent işlenir.
- Process death sonrasında sonraki sync hakkı geri bulur.
- Abonelik renewal/grace/account-hold state'leri V1 kapsamından çıkarılmıştır; çünkü abonelik yoktur.

Ayrıntı: `BILLING_RESTORE_SPEC.md` ve `PRODUCT_MODEL_V1.md`.

## 7. Merkezi config

```text
rewardedAdsPerUnlock = 2
timedInterstitialEligibilitySeconds = 90
premiumProductType = nonConsumable
premiumDisplayName = LP FAL Premium
```

Banner placement, App Open, interstitial gating ve Premium ad suppression merkezi `MonetizationConfig/AdPolicy` üzerinden yönetilir; ekranlara magic number/string dağılmaz.

## 8. İsim standardı

Premium ürünü için kullanıcıya görünen hiçbir yerde:
- `PRO`
- `Pro`
- `Ömür Boyu Premium`
- `Lifetime Premium`
kullanılmaz.

Doğru ad: **Premium / LP FAL Premium**.

## 9. Compliance bağlantısı

Reklam ve satın alma UI'ı `COMPLIANCE_BY_DESIGN.md` kurallarına da tabidir:
- dark pattern yok,
- sistem uyarısını taklit eden reklam yok,
- yanlış tıklama teşvik eden yerleşim yok,
- fal/rüya AI içeriği reklam hedefleme segmentine dönüşmez,
- store listing ile gerçek Premium/reklam davranışı çelişmez.

## 10. Release QA

- [ ] Kahve/Tarot/Rüya/El/chat unlock için 2 Rewarded gerekiyor.
- [ ] Tek rewarded unlock vermiyor.
- [ ] Rüya yazma/analiz/sonuç/chat sırasında timed interstitial yok.
- [ ] 90 saniyeden önce timed interstitial uygunluğu oluşmuyor.
- [ ] 90 saniye dolunca reklam anında zorla açılmıyor.
- [ ] Timed interstitial yalnız doğal geçişte gösteriliyor.
- [ ] Kahve/El capture veya AI inference sırasında timed ad yok.
- [ ] Rewarded transaction ile timed interstitial çakışmıyor.
- [ ] Phone banner interaktif öğelerden güvenli mesafede.
- [ ] Tablet/BlueStacks side banner ana içeriği bozmuyor.
- [ ] Dream/chat/analysis/capture/result ekranlarında banner yok.
- [ ] Premium aktifken hiçbir reklam türü ve boş reklam container'ı görünmüyor.
- [ ] Premium aktifken yeni ad request yok.
- [ ] Test build'lerinde yalnız AdMob test ID'leri kullanılıyor.
- [ ] Premium tek seferlik satın alım olarak çalışıyor; subscription ürünü yok.
- [ ] Reinstall/new-device/manual restore başarılı.
- [ ] Premium UI metninde `PRO/Pro` yok.
- [ ] Reklam sistemi `COMPLIANCE_BY_DESIGN.md` dark-pattern ve sensitive-content kurallarını geçiyor.

**Bu dosya V1 final kontrolünün zorunlu girdisidir.**