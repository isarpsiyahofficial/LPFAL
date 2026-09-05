# LP FAL — V1 Monetizasyon Politikası

**Durum:** ZORUNLU / normatif  
**Öncelik:** Reklam zamanlaması ve yerleşimi konusunda bu dosya `SPECIFICATION.md`, `TODO.md` ve `V1_RELEASE_GATES.md` içindeki eski varsayımların yerine geçer.

## 1. Premium kullanıcı

Premium aktifse reklam sistemi tamamen kapalıdır:
- Rewarded yok.
- Süreye dayalı interstitial yok.
- App Open yok.
- Banner/native yok.
- Reklam request'i mümkün olduğunca oluşturulmaz.

## 2. Rewarded unlock

Ücretsiz kullanıcı:
- Kahve falı tam sonucu: **2 Rewarded Ad**.
- Tarot tam yorumu: **2 Rewarded Ad**.
- Fal sohbetinde yeni mesaj paketi: **2 Rewarded Ad**.

Kurallar:
- `0/2 → 1/2 → 2/2` ilerlemesi gösterilir.
- İki reklam ayrı ayrı kullanıcı tarafından başlatılır.
- İkinci reklam otomatik açılmaz.
- Tek reklam reward vermez.
- Reward yalnız iki başarılı `reward earned` callback'inden sonra açılır.

## 3. Süreye dayalı interstitial — 90 saniye

V1 sabiti: `timedInterstitialEligibilitySeconds = 90`

90 saniye zorla reklam açma süresi değildir; yalnız yeni interstitial için uygunluk üretir.

- Yalnız foreground + aktif kullanım süresi sayılır.
- Background, reklam izleme ve model indirme süresi sayılmaz.
- 90 saniye dolunca `timedInterstitialEligible = true` olur.
- Reklam yalnız sonraki doğal/güvenli ekran geçişinde gösterilir.
- Gösterim sonrası sayaç sıfırlanır ve yeni 90 saniye dönemi başlar.
- Premium kullanıcıda timer çalışmaz.

Timed interstitial şu anlarda gösterilmez:
- kahve fotoğrafı seçme/çekme,
- Qwen analiz/inference,
- fal sonucunu aktif okuma,
- tarot kart seçme/çevirme,
- chat mesaj yazma veya AI cevap üretme,
- rewarded transaction,
- rewarded reklamın hemen öncesi/sonrası,
- billing/consent/permission akışları.

## 4. Banner reklamlar

### Telefon / portrait
- Ana format: **anchored adaptive banner**.
- Banner yalnız güvenli ekranlarda üst veya alt bölgede gösterilir.
- Tercih edilen yüzeyler: Dashboard, Fallarım/Geçmiş, Profil/Ayarlar.
- Alt banner kullanılacaksa bottom navigation ile arasında açık, tıklanamaz bir ayırıcı/spacer bulunur.
- Üst banner kullanılacaksa app bar/CTA ile arasında yeterli boşluk bulunur.
- Aynı telefonda eşzamanlı hem üst hem alt banner gösterilmez.

### Tablet / BlueStacks / geniş ekran
- İçeriği daraltmadan sağ veya sol reklam kolonu kullanılabilir.
- Reklam kolonu uygulamanın ana içerik kartlarından görsel olarak ayrılır.
- V1 varsayılanı tek yan kolondur; iki yan reklam yalnız geniş ekran QA ve politika kontrolünden sonra değerlendirilebilir.

### Banner gösterilmeyecek ekranlar
- Kahve fotoğrafı çekme/seçme.
- Qwen analiz/loading.
- Fal chatbot ekranı.
- Tarot kart seçme/çevirme.
- Rewarded akışı.
- Premium/billing checkout.
- Permission/consent modal akışları.

### Banner kalite kuralları
- Reklam ile CTA, bottom navigation, chat input, fotoğraf galerisi ve diğer etkileşimli öğeler arasında yeterli ayırıcı alan bulunur.
- Banner hiçbir butonun parçası gibi görünmez.
- Banner fal metninin içine veya sohbet mesajları arasına yerleştirilmez.
- Premium aktifse banner container tamamen kaldırılır; boş reklam alanı bırakılmaz.

## 5. App Open

- App Open timed interstitial'dan ayrı mekanizmadır.
- İlk açılışlarda agresif kullanılmaz.
- Rewarded/timed interstitial ile çakışmaz.
- Premium'da kapalıdır.

## 6. Merkezi config

```text
rewardedAdsPerUnlock = 2
timedInterstitialEligibilitySeconds = 90
```

Banner placement, App Open ve interstitial gating merkezi `MonetizationConfig/AdPolicy` üzerinden yönetilir; ekranlarda magic number kullanılmaz.

## 7. Release QA

- [ ] 90 saniyeden önce timed interstitial uygunluğu oluşmuyor.
- [ ] 90 saniye dolunca reklam anında zorla açılmıyor.
- [ ] Timed interstitial yalnız doğal geçişte gösteriliyor.
- [ ] Rewarded transaction ile timed interstitial çakışmıyor.
- [ ] Phone banner interaktif öğelerden güvenli mesafede.
- [ ] Tablet/BlueStacks yan banner ana içeriği bozup taşırmıyor.
- [ ] Chat input yakınında banner yok.
- [ ] Premium aktifken hiçbir reklam türü ve boş reklam container'ı görünmüyor.
- [ ] Test build'lerinde yalnız AdMob test ID'leri kullanılıyor.

**Bu dosya V1 final kontrolünün zorunlu girdisidir.**