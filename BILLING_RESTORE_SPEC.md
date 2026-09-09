# LP FAL — Google Play Billing / Restore Şartname Eki

**Durum:** ZORUNLU / normatif  
**Kapsam:** V1 aylık Premium abonelik, Google Play restore/senkronizasyonu ve Premium–reklam entegrasyonu  
**Referans yaklaşım:** `isarpsiyahofficial/MIZANGLOBAL` monetizasyon mimarisi yalnız **okuma/referans** amacıyla incelenmiştir. MIZAN reposunun yapısı değiştirilmeyecek ve LP FAL için MIZAN'ın ürün kimlikleri veya lifetime mantığı kopyalanmayacaktır.

---

## 1. Ürün modeli — LP FAL için kilitli

LP FAL V1'de tek ücretli ürün:
- **Aylık otomatik yenilenen Premium abonelik**.
- Premium'un zorunlu faydası: **uygulamadaki tüm reklamların kaldırılması**.
- V1'de lifetime / non-consumable Premium yoktur.
- V1'de birden fazla Premium katmanı yoktur.
- Google Play Console'daki gerçek ürün/base-plan kimliği uygulama implementasyonu sırasında tek merkezden tanımlanır; UI içine magic string dağıtılmaz.
- Fiyat, para birimi, faturalandırma dönemi ve Play tarafından sağlanan ürün metadatası mümkün olduğunca `ProductDetails`/Billing bilgisinden gösterilir; `₺49,99` gibi sabit fiyat kodlanmaz.

`design_refs` altındaki bir mockup'ta lifetime veya ek Premium özellikleri görünmesi ürün kapsamını değiştirmez; mockup yalnız görsel referanstır.

---

## 2. MIZAN'dan alınacak mimari davranış

MIZAN'da çalışan yaklaşım LP FAL'e şu prensiplerle uyarlanır:

1. Satın alma servisi uygulama yaşam döngüsünden ayrılmış merkezi bir servis olur.
2. `purchaseStream` dinleyicisi satın alma/restore sonuçlarını tek yerde işler.
3. Google Play store erişilebilirliği ve ürün metadatası merkezi olarak yüklenir.
4. Uygulama açılışında mevcut satın alma hakkı sessizce senkronize edilir.
5. Uygulama foreground'a döndüğünde Premium hakkı yeniden kontrol edilir.
6. İnternet yeniden geldiğinde mevcut Play hakkı yeniden senkronize edilir.
7. Purchase ve restore aynı entitlement doğrulama hattından geçer.
8. Yerel Premium cache'i yalnız son bilinen UX state'idir; tek başına ücretli Premium üretmez.
9. Premium state değişince reklam servisi merkezi olarak yeniden değerlendirilir.
10. Premium aktif olduğunda yüklenmiş reklam nesneleri de dispose edilir; yeni reklam request'i oluşturulmaz.

MIZAN'daki `premium_lifetime`, local promotion, günlük rewarded-Premium süresi ve MIZAN'a özel reklam sayıları LP FAL'e kopyalanmaz.

---

## 3. Satın alma servisi

Önerilen katmanlar:
- `PurchaseService`
- `PremiumEntitlementStore`
- `MonetizationController`
- `AdService`
- `AdPolicy / MonetizationConfig`

### Başlatma sırası
1. Uygulamanın gerekli hukuki/onay ekranları tamamlanır.
2. `purchaseStream` listener bağlanır.
3. Google Play Billing kullanılabilirliği kontrol edilir.
4. Aylık Premium ürün metadatası yüklenir.
5. Mevcut satın alımlar/abonelik hakkı sessizce senkronize edilir.
6. Entitlement snapshot güncellenir.
7. Premium ise reklam sistemi suppress edilir; Free ise consent sonrası reklam servisi hazırlanabilir.

Aynı initialization/sync işleminin paralel iki kez çalışması engellenir. Restore/sync idempotent olmalıdır.

---

## 4. Google Play restore / entitlement source of truth

### Otomatik restore
Kullanıcı normal şartlarda ayrıca restore butonuna basmak zorunda kalmamalıdır. Aşağıdaki anlarda sessiz senkronizasyon yapılır:
- clean install / ilk uygun açılış,
- uygulama açılışı,
- app resume,
- internet bağlantısının geri gelmesi,
- satın alma callback'i sonrası,
- process death sonrası sonraki açılış/resume.

Android'de mevcut/owned Play satın alımları güncel desteklenen Billing API / Flutter `in_app_purchase` Android katmanı üzerinden sorgulanır. Restore edilen satın alma bilgileri de aynı `purchaseStream`/validation hattında işlenir.

### Manuel restore
Premium veya Ayarlar ekranında kullanıcı desteği için:
- **`Satın Alımları Geri Yükle`** aksiyonu bulunur.
- Buton otomatik senkronizasyonun alternatifi değil, kullanıcı tarafından tetiklenen ek recovery yoludur.
- Zaten Premium olan kullanıcıda tekrar entitlement üretmez; işlem idempotenttir.

### Aynı Google hesabı / yeni cihaz
Aynı Google Play hesabında aktif aylık aboneliği olan kullanıcı uygulamayı başka desteklenen Android cihaza kurduğunda store senkronizasyonu Premium'u yeniden tanımalıdır.

---

## 5. Entitlement güven kuralları

- `isPremium=true` şeklinde tek başına değiştirilebilir yerel boolean **source of truth değildir**.
- Ücretli Premium için Play tarafından dönen satın alma/abonelik kanıtı ve güncel owned-entitlement senkronizasyonu gerekir.
- Purchase token / verification data boşsa ücretli Premium verilmez.
- Aynı purchase token için tekrarlanan callback entitlement'ı iki kez üretmez.
- İstenirse MIZAN'daki yaklaşıma benzer şekilde product ID + purchase token materyalinden SHA-256 fingerprint tutularak yerel state'in bütünlük bağı güçlendirilir.
- Fingerprint yalnız cache/bütünlük sinyalidir; Google Play state'inin yerine geçmez.
- Purchase error veya user cancel Premium vermez.
- Initial purchase `pending` durumundayken ödeme tamamlanmadan Premium açılmaz.
- `pendingCompletePurchase`/acknowledgement gereken satın alımlar desteklenen Billing akışına göre tamamlanır.
- Tam backend doğrulaması bulunmayan V1'de mutlak anti-tamper iddiası yapılmaz; online durumda Google Play store state'i otoritedir.

---

## 6. Aylık abonelik yaşam döngüsü

Uygulama state matrisi:

| Google Play durumu | LP FAL Premium | Reklamlar |
| --- | --- | --- |
| İlk satın alma pending | Kapalı | Free kuralları |
| Satın alma aktif | Açık | Tamamen kapalı |
| Normal yenileme aktif | Açık | Tamamen kapalı |
| Grace period | Açık | Tamamen kapalı |
| Kullanıcı iptal etti fakat paid-through-end devam ediyor | Bitiş tarihine kadar açık | Bitiş tarihine kadar kapalı |
| Account hold | Kapalı | Free kuralları geri döner |
| Expired | Kapalı | Free kuralları geri döner |
| Revoked / refunded ve artık entitled değil | Kapalı | Free kuralları geri döner |

Uygulama kendi başına `grace` veya `hold` uydurmaz; güncel Play entitlement davranışını esas alır.

---

## 7. Offline davranış

- Son doğrulanmış aktif Premium state'i kısa süreli/offline UX için yerel snapshot olarak tutulabilir.
- Yerel cache kalıcı source of truth değildir.
- Bağlantı geri geldiğinde sessiz Play sync yapılır ve cache store sonucuna göre düzeltilir.
- Süresi dolmuş/hold olmuş abonelik eski bir yerel boolean yüzünden sonsuza kadar Premium kalamaz.
- Offline state'te ödeme/restore başlatılamıyorsa kullanıcıya anlaşılır hata gösterilir; uygulama Premium'u tahmin ederek açmaz.

---

## 8. Premium → reklam suppression

Premium aktif olduğunda tek merkezi controller üzerinden:
- Rewarded kapalı.
- Timed interstitial kapalı ve timer devre dışı.
- App Open kapalı.
- Banner/native kapalı.
- Yüklü interstitial/rewarded/App Open/banner nesneleri dispose edilir.
- Yeni AdMob request'i oluşturulmaz.
- Banner/rail container UI'dan tamamen kaldırılır; boş alan bırakılmaz.

Premium entitlement kaybedildiğinde:
- reklam sistemi billing/restore ekranının ortasında aniden açılmaz,
- önce entitlement state kesinleşir,
- UMP/consent şartları sağlanır,
- reklamlar yalnız güvenli/natural UI noktalarında yeniden hazırlanır.

MIZAN'daki `setPremiumSuppressed(true) -> disposeLoadedAds()` prensibi LP FAL'de tüm kullanılan reklam formatlarını kapsayacak şekilde uygulanır.

---

## 9. Free reklam kuralları değişmez

Bu restore mimarisi LP FAL'in mevcut reklam sayılarını değiştirmez:
- Kahve tam sonuç: 2 Rewarded.
- Tarot tam yorum: 2 Rewarded.
- El Falı tam yorum: 2 Rewarded.
- Gated chat paketi: 2 Rewarded.
- Timed interstitial eligibility: 90 saniye aktif foreground kullanım + doğal geçiş.
- Banner/App Open kuralları `MONETIZATION_V1.md` içindeki şekliyle geçerlidir.

MIZAN'daki 3 rewarded / 60 saniye gibi MIZAN'a özel sabitler LP FAL'e taşınmaz.

---

## 10. AdMob/consent yapı prensibi

MIZAN'daki güvenli desen LP FAL'e uyarlanır:
- UMP consent çözülmeden reklam request'i yapılmaz.
- Premium suppress edilmişse Mobile Ads gereksiz yere initialize edilmez.
- Aynı anda birden fazla full-screen reklam gösterilmez.
- Reward yalnız gerçek `onUserEarnedReward` callback'i ile sayılır.
- Ad dismissal/failure sonrasında ilgili ad dispose edilir.
- Free kullanıcı için gerekli reklamlar kontrollü biçimde preload edilebilir.
- Test build yalnız Google test ad unit ID'leri kullanır.
- Production ID'leri merkezi config / build environment üzerinden gelir.
- Production build'de test ID veya boş/bozuk production ID tespit edilirse release gate fail olur.

---

## 11. Premium ekranı

Premium ekranında gerçek Play verisi kullanılacaktır:
- Aylık Premium ürün adı/fiyatı.
- Otomatik yenileme bilgisi.
- Aboneliği iptal etmenin bir sonraki yenilemeyi durdurduğu; mevcut paid-through-end hakkın Play state'ine göre devam edeceği bilgisi.
- `Satın Al`.
- `Satın Alımları Geri Yükle`.
- `Aboneliği Yönet` → Google Play abonelik yönetimi.
- Privacy / Terms / gerekiyorsa satın alma koşulları erişimi.

V1 aylık-only olduğu için Premium ekranında lifetime satın alma gösterilmez.

---

## 12. Process death / recovery

Aşağıdaki senaryoda satın alma kaybolmuş sayılmaz:
1. Kullanıcı Play ödeme ekranını açar.
2. Satın alma gerçekleşir veya pending olur.
3. Android uygulama process'ini öldürür / uygulama kapanır.
4. Sonraki açılışta purchase listener + owned-purchase sync store state'ini tekrar okur.
5. Entitlement doğru state'e getirilir.

UI callback'inin kaçırılması tek başına Premium kaybına yol açamaz.

---

## 13. Blocking QA matrisi

Aşağıdakiler test edilmeden Premium fazı final sayılmaz:
- [ ] Yeni aylık satın alma başarılı → Premium anında/Play callback sonrası aktif.
- [ ] Pending satın alma → ödeme tamamlanmadan Premium yok.
- [ ] Kullanıcı purchase sheet'i iptal etti → Premium yok.
- [ ] Clean install + aynı Google hesabında aktif abonelik → otomatik restore.
- [ ] App data clear/reinstall sonrası aktif abonelik → restore.
- [ ] Manuel `Satın Alımları Geri Yükle` → doğru state.
- [ ] Zaten aktif Premium'da restore → duplicate state yok.
- [ ] App resume → silent sync.
- [ ] Offline → online reconnect → silent sync.
- [ ] Process death satın alma sırasında → sonraki launch state recovery.
- [ ] Yenileme → Premium kesilmiyor.
- [ ] Grace period → Premium/reklamsız devam ediyor.
- [ ] Cancelled-but-paid-through-end → bitişe kadar Premium devam ediyor.
- [ ] Account hold → Premium kapanıyor.
- [ ] Expired → Premium kapanıyor.
- [ ] Revoked/refunded entitlement yok → Premium kapanıyor.
- [ ] Duplicate purchase stream callback → idempotent.
- [ ] Invalid/empty Play verification data → Premium verilmez.
- [ ] Premium aktif olur olmaz yüklü reklamlar dispose ediliyor.
- [ ] Premium aktifken sıfır Rewarded/interstitial/App Open/banner request/container.
- [ ] Premium bittiğinde reklamlar billing ekranının ortasında patlamıyor; güvenli akışta geri geliyor.
- [ ] Store fiyatı UI'da Play `ProductDetails` üzerinden geliyor.
- [ ] Debug/test build yalnız test AdMob ID.
- [ ] Release build yalnız doğrulanmış production AdMob ID.

---

## 14. MIZAN güvenlik sınırı

Bu şartname hazırlanırken MIZANGLOBAL yalnız okunmuştur. LP FAL çalışması sırasında:
- MIZAN dosyaları değiştirilmez,
- MIZAN branch/ref'i ilerletilmez,
- MIZAN ürün kimliği LP FAL'e kopyalanmaz,
- MIZAN'ın lifetime Premium davranışı LP FAL'e taşınmaz.

Alınan şey yalnız test edilmiş **mimari desen**dir: `purchase stream → Play sync/restore → entitlement snapshot → merkezi ad suppression`.
