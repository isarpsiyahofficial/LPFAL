# LP FAL — V1 Monetizasyon Politikası

**Durum:** ZORUNLU / normatif  
**Öncelik:** Reklam zamanlaması ve yerleşimi konusunda bu dosya ana kaynaktır.

## 1. Premium kullanıcı

Premium aktifse reklam sistemi tamamen kapalıdır:
- Rewarded yok.
- Süreye dayalı interstitial yok.
- App Open yok.
- Banner/native yok.
- Reklam request'i mümkün olduğunca oluşturulmaz.
- Kahve, Tarot, El Falı ve fal sohbeti reklam beklemeden kullanılır.

## 2. Rewarded unlock

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
- Qwen analiz/inference,
- fal sonucunu aktif okuma,
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
- Qwen analiz/loading.
- Fal chatbot ekranı.
- Tarot kart seçme/çevirme.
- El analiz/sonuç üretim state'i.
- Rewarded akışı.
- Premium/billing checkout.
- Permission/consent modal akışları.

### Banner kalite kuralları
- CTA, bottom navigation, chat input, galeriler ve diğer etkileşimli öğelerden güvenli mesafede.
- Banner hiçbir butonun parçası gibi görünmez.
- Fal metninin içine veya sohbet mesajları arasına yerleştirilmez.
- Premium aktifse banner container tamamen kaldırılır.

## 5. App Open

- Timed interstitial'dan ayrı mekanizma.
- İlk açılışlarda agresif kullanılmaz.
- Rewarded/timed interstitial ile çakışmaz.
- Kullanıcı kritik capture/analysis/billing akışına dönüyorsa gösterilmez.
- Premium'da kapalıdır.

## 6. Merkezi config

```text
rewardedAdsPerUnlock = 2
timedInterstitialEligibilitySeconds = 90
```

Banner placement, App Open ve interstitial gating merkezi `MonetizationConfig/AdPolicy` üzerinden yönetilir; ekranlara magic number dağılmaz.

## 7. Release QA

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
- [ ] Premium aktifken hiçbir reklam türü ve boş reklam container'ı görünmüyor.
- [ ] Test build'lerinde yalnız AdMob test ID'leri kullanılıyor.

**Bu dosya V1 final kontrolünün zorunlu girdisidir.**