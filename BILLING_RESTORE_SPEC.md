# LP FAL — Google Play Satın Alma / Restore Şartname Eki

**Durum:** ZORUNLU / normatif  
**Kapsam:** V1 tek seferlik Premium satın alımı, Google Play otomatik restore/senkronizasyonu ve Premium–reklam entegrasyonu  
**Ürün modeli ana kaynağı:** `PRODUCT_MODEL_V1.md`

---

## 1. Ürün modeli — kilitli

LP FAL V1'de tek ücretli ürün **LP FAL Premium**'dur.

- Aylık/yıllık/otomatik yenilenen abonelik yoktur.
- Google Play üzerinden tek seferlik kalıcı Premium satın alımı vardır.
- Kullanıcıya görünen paket adı `Ömür Boyu`, `Lifetime`, `Permanent` veya `PRO` olmayacaktır.
- Ekranda plan adı yalnız **LP FAL Premium** / **Premium** olarak gösterilir.
- Açıklama: **Tek seferlik satın alım · Abonelik değildir.**
- Fiyat ve para birimi Google Play ürün bilgisinden gösterilir; hard-code edilmez.
- Google Play Console ürün kimliği tek merkezi config'te tutulur; UI içine magic string dağılmaz.
- Restore kullanıcıya gösterilen bir özellik değildir; tamamen arka planda çalışır.

---

## 2. MIZAN'dan referans alınan mimari davranış

MIZAN reposu yalnız okuma/referans amacıyla incelenmiştir; yapısı değiştirilmez.

LP FAL'e taşınacak prensipler:
1. Satın alma servisi merkezi ve UI'dan ayrıdır.
2. `purchaseStream` satın alma/restore sonuçlarını tek yerde işler.
3. Store availability ve product details merkezi yüklenir.
4. Uygulama açılışında mevcut owned purchase sessizce senkronize edilir.
5. App resume ve internet geri gelişi sırasında entitlement yeniden kontrol edilir.
6. Purchase ve restore aynı doğrulama hattından geçer.
7. Yerel Premium cache'i yalnız son bilinen UX state'idir; tek başına ücretli Premium üretmez.
8. Premium state değişince reklam servisi merkezi yeniden değerlendirilir.
9. Premium aktif olduğunda yüklenmiş reklamlar dispose edilir ve yeni reklam request'i yapılmaz.
10. Aynı satın alma tekrar geldiğinde idempotent davranılır.
11. Restore/sync kullanıcı etkileşimi istemez ve görünür UI üretmez.

MIZAN'a özel ürün adı, promosyon mantığı, günlük geçici Premium ve MIZAN reklam sayıları LP FAL'e kopyalanmaz.

---

## 3. Önerilen katmanlar

- `PurchaseService`
- `PremiumEntitlementStore`
- `MonetizationController`
- `AdService`
- `AdPolicy / MonetizationConfig`

### Başlatma sırası
1. Gerekli hukuki/onay ekranları tamamlanır.
2. `purchaseStream` listener bağlanır.
3. Google Play Billing kullanılabilirliği kontrol edilir.
4. LP FAL Premium ürün metadatası yüklenir.
5. Mevcut owned purchases sessizce senkronize edilir.
6. Entitlement snapshot güncellenir.
7. Premium ise reklam sistemi suppress edilir; Free ise consent sonrası reklam servisi hazırlanabilir.

Aynı initialization/sync işleminin paralel iki kez çalışması engellenir. Restore/sync idempotent olmalıdır.

---

## 4. Satın alma akışı

1. Kullanıcı Premium ekranını açar.
2. Google Play'den gerçek ürün fiyatı/para birimi yüklenir.
3. Kullanıcı `Premium'a Geç` / `Premium'u Aç` butonuna basar.
4. Google Play satın alma akışı başlar.
5. Pending durum Premium vermez.
6. Purchased/restored state geldiğinde purchase proof doğrulanır.
7. Doğrulama başarılıysa Premium entitlement yazılır.
8. Gerekliyse satın alma acknowledgement/complete işlemi tamamlanır.
9. Premium state değişir değişmez bütün reklam sistemi suppress edilir.
10. UI `Premium Aktif` state'ine geçer.

Başarısız/cancelled satın alma ücretsiz entitlement üretmez.

---

## 5. Google Play restore / entitlement source of truth

### Otomatik ve görünmez restore
Kullanıcı restore diye bir özellik görmez ve ayrıca hiçbir butona basmaz. Aşağıdaki anlarda sessiz senkronizasyon yapılır:
- clean install / ilk uygun açılış,
- normal uygulama açılışı,
- app resume,
- internet bağlantısının geri gelmesi,
- satın alma callback'i sonrası,
- process death sonrası sonraki açılış/resume.

Android'de owned Play satın alımı güncel desteklenen Billing API / Flutter `in_app_purchase` Android katmanı üzerinden sorgulanır. Restore edilen satın alma bilgileri aynı validation hattında işlenir.

### UI kuralı
- Premium ekranında restore butonu YOK.
- Ayarlar ekranında restore butonu YOK.
- `Satın Alımı Geri Yükle`, `Restore`, `Geri Yükle` gibi teknik kullanıcı metinleri YOK.
- Otomatik restore/sync loading spinner, popup veya ayrı status ekranı üretmez.
- Restore başarısızsa kullanıcı normal akışta kalır; sistem sonraki uygun lifecycle/network olayında sessizce tekrar dener.

### Clean install / yeni cihaz
Aynı Google Play hesabında ürün hâlâ owned ise:
- tekrar ödeme istenmez,
- sessiz sync Premium'u otomatik geri getirir.

---

## 6. Entitlement güveni

- Yerel `isPremium=true` flag'i tek başına güvenilir ücretli hak değildir.
- Google Play kaynaklı valid purchase proof/token bulunmadan ücretli Premium oluşturulmaz.
- Purchase fingerprint/token material doğrudan kullanıcıya gösterilmez.
- Gerekli yerel fingerprint/hash yalnız entitlement bütünlüğü/idempotency için kullanılabilir.
- Aynı token/purchase birden fazla kez gelirse tekrar tekrar hak üretmez.
- Ürün ID eşleşmesi zorunludur.
- Başka ürün ID'si LP FAL Premium entitlement veremez.

---

## 7. Process-death recovery

Satın alma sırasında uygulama öldürülürse:
- callback kaybı Premium hakkını kalıcı olarak kaybettirmez,
- sonraki launch/resume'da owned purchase sync yapılır,
- valid ürün bulunursa Premium otomatik restore edilir,
- duplicate callback sorun yaratmaz.

---

## 8. Offline davranış

Premium daha önce valid Google Play hakkıyla doğrulanmışsa:
- uygulama kısa süreli offline kullanımda son doğrulanmış Premium snapshot'ını UX için kullanabilir,
- bu cache yeni Premium üretmek için kullanılamaz,
- internet geldiğinde Play sync tekrar yapılır.

Free kullanıcı interneti kapatarak yeni ücretli Premium oluşturamaz.

---

## 9. Premium → reklam suppression

Premium aktif olduğunda merkezi monetizasyon katmanı:
- Rewarded yükleme/gösterimi kapatır,
- interstitial yükleme/gösterimi kapatır,
- App Open kapatır,
- banner/native kapatır,
- yüklenmiş full-screen reklamları dispose eder,
- banner container'larını kaldırır,
- yeni reklam request'i üretmez,
- 90 saniye timed interstitial timer'ını durdurur/sıfırlar.

Premium state değişimi billing ekranının ortasında kullanıcıya reklam göstermemelidir.

---

## 10. UI metin standardı

Kullanıcıya görünen doğru metinler:
- `LP FAL Premium`
- `Premium`
- `Premium'a Geç`
- `Premium Aktif`
- `Tek seferlik satın alım · Abonelik değildir.`

Kullanılmayacak:
- `PRO`
- `Pro`
- `Ömür Boyu Premium`
- `Lifetime Premium`
- `Aylık Premium`
- `Aboneliği Yönet`
- `Satın Alımı Geri Yükle`
- `Restore`
- `Geri Yükle`

---

## 11. Hata durumları

UI en az şu satın alma state'lerini ayırabilmelidir:
- store unavailable,
- product unavailable,
- purchase pending,
- purchase canceled,
- purchase error,
- invalid purchase proof,
- acknowledgement/complete error,
- already owned / Premium already active.

Restore/sync hatası teknik olarak loglanabilir fakat kullanıcıya restore özelliği olarak sunulmaz. Hiçbir hata uygulamayı crash/freeze etmemelidir.

---

## 12. Release test matrisi — bloklayıcı

- [ ] İlk satın alma başarılı → Premium aktif.
- [ ] Pending ödeme Premium vermiyor.
- [ ] Cancel akışı Premium vermiyor.
- [ ] Aynı purchase callback iki kez gelince duplicate hak yok.
- [ ] Uygulama satın alma sırasında öldürülüp yeniden açılınca Premium otomatik restore ediliyor.
- [ ] Clean install sonrası aynı Play hesabında Premium otomatik geri geliyor.
- [ ] Yeni cihaz senaryosunda Premium otomatik geri geliyor.
- [ ] Kullanıcı UI'ında restore/geri yükleme butonu yok.
- [ ] İnternet geri gelince sessiz sync çalışıyor.
- [ ] App resume sync çalışıyor.
- [ ] Geçersiz/boş Play proof Premium vermiyor.
- [ ] Yanlış product ID Premium vermiyor.
- [ ] Premium aktifken rewarded/interstitial/App Open/banner/native request yok.
- [ ] Premium aktifken boş reklam container yok.
- [ ] Premium ekranında fiyat hard-code değil, Google Play'den geliyor.
- [ ] Uygulamada subscription ürünü/base-plan mantığı yok.
- [ ] Kullanıcı UI'ında Premium özelliği için `PRO/Pro` yok.
- [ ] `Ömür Boyu` / `Lifetime` paket adı yok.

**Bu testlerden biri başarısızsa Premium sistemi final kabul edilmez.**