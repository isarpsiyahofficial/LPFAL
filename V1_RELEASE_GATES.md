# LP FAL — V1 Ek Release Gates

**Durum:** ZORUNLU / normatif  
**Bağlı dosyalar:** `SPECIFICATION.md`, `TODO.md`, `MONETIZATION_V1.md`, `BILLING_RESTORE_SPEC.md`

Bu dosyadaki maddeler V1 final kontrolünün zorunlu parçasıdır.

---

## 1. Kapsam kilidi

V1 fal türleri yalnız:
- Kahve Falı,
- Tarot Falı,
- El Falı.

V1'e burç/astroloji, rüya, numeroloji, sosyal özellik, coin/kredi veya genel AI asistanı eklenmez.

V1 kullanıcı arayüzü ve fal üretim dili Türkçe'dir; kod i18n'e hazır olabilir.

---

## 2. Ortak güvenli yorum release gate'i

Kahve, Tarot, El ve Fal Sohbeti için:
- kesin gelecek iddiası yok,
- kullanıcıya önemli hayat kararı aldıran emir/tavsiye yok,
- sağlık teşhisi yok,
- ölüm/yaşam süresi tahmini yok,
- hamilelik/doğurganlık kesinliği yok,
- hukuki sonuç garantisi yok,
- yatırım/şans/finansal kazanç garantisi yok.

Sonuç dili sembolik ve koşullu olmalıdır: `çağrıştırabilir`, `geleneksel yorumlarda`, `sembolik olarak`.

Her fal sonucu görünür entertainment/professional-advice disclaimer içerir.

Safety yalnız system prompt'a bırakılmaz; uygulama seviyesinde output kontrolü bulunur.

---

## 3. Fotoğraf ve veri gizliliği

Kahve + El için:
- System Photo Picker/scoped yaklaşım tercih edilir.
- Broad storage permission mümkün olduğunca yok.
- Kamera izni yalnız kullanıcı capture başlatınca.
- Fotoğraf app-private geçici alanda.
- Ham görüntüler varsayılan inference sonrası silinir.
- Production loglarında fotoğraf path/prompt/chat/structured-analysis yok.
- Fotoğraf/chat/fal verileri varsayılan eğitim verisine dönüşmez.
- Auto Backup ile ham fotoğraf/model/AI cache izinsiz buluta gitmez.

---

## 4. Kahve görsel kalite gate'i

- 2–3 fotoğraf tek `merged cup analysis` üretir.
- Aynı sembol farklı açılarda duplicate sayılmaz.
- Çelişkili bulgu uncertain/elenmiş state'e alınır.
- Qwen self-reported confidence gerçek olasılık kabul edilmez.
- Threshold validation setiyle kalibre edilir.
- En az 100 gerçek fincan fotoğrafı/grubu QA seti.
- Negatif ve zor görüntüler dahil.
- Hedef high-confidence precision ≥ %80.
- Hedef high-confidence false-positive ≤ %15.

---

## 5. Tarot draw gate'i

- 78 kart metadata + asset mapping eksiksiz.
- Aynı açılımda aynı kart iki kez yok.
- Draw uygulama engine'i tarafından güvenilir RNG ile yapılır.
- Qwen kart seçmez ve kartı görselden yeniden tanımaz.
- Kart ID/ad/pozisyon/upright-reversed structured verilir.
- 1/3/5 kart açılımları testli.

---

## 6. El Falı özel güvenlik ve doğruluk gate'i

El Falı yalnız geleneksel palmistry sembolizmini eğlence amaçlı yorumlar.

Zorunlu:
- el/avuç varlık ve kalite kontrolü,
- yalnız görünür çizgi/bölge analizi,
- görünmeyen çizgi için `not_visible/uncertain`,
- structured palm analysis → sembolik yorum iki aşaması,
- confidence validation ile kalibre,
- en az 100 gerçek avuç görüntüsü/grubu test seti,
- farklı ışık, ten tonu, kamera ve poz çeşitliliği.

Kesin yasak:
- fingerprint template,
- biyometrik kimlik doğrulama/eşleştirme,
- kişiyi benzersiz tanıma,
- sağlık/hastalık çıkarımı,
- ölüm/yaşam süresi çıkarımı,
- hamilelik/doğurganlık çıkarımı,
- ırk/etnik köken, din, siyasi görüş, cinsel yönelim gibi hassas özellik çıkarımı,
- gereksiz yaş/cinsiyet tahmini,
- deterministik kişilik hükmü.

---

## 7. AI sürümleme ve on-device gate'i

Her release kaydı:
- Qwen model/revision,
- quantization,
- model SHA-256,
- vision projector/encoder SHA-256 varsa,
- runtime sürümü,
- coffee/tarot/palm/chat/safety prompt sürümleri,
- generation parametreleri.

Model veya prompt değişince regresyon tekrar çalışır.

AI inference için fotoğraf/prompt/chat sunucuya gönderilmez. Cloudflare/harici inference yok.

Debug model Play olmadan local kurulabilir. Release model base APK içine gömülmez; release tarihindeki Play on-device/asset delivery yöntemi yeniden doğrulanır.

---

## 8. Monetizasyon gate'i

`MONETIZATION_V1.md` bu konuda ana kaynaktır.

### Rewarded
Ücretsiz kullanıcıda:
- Kahve full result = 2 Rewarded,
- Tarot full result = 2 Rewarded,
- El Falı full result = 2 Rewarded,
- gated chat pack = 2 Rewarded.

- UI `0/2 → 1/2 → 2/2`.
- Her reklam ayrı kullanıcı opt-in.
- İlk reklam tek başına entitlement vermez.
- İkinci başarılı `reward earned` callback gelmeden unlock yok.
- Ad failure ücretsiz entitlement üretmez.

### Timed interstitial
V1 sabiti:
`timedInterstitialEligibilitySeconds = 90`

- Yalnız foreground aktif kullanım sayılır.
- 90 sn dolması sadece eligibility oluşturur.
- Reklam ancak sonraki güvenli/doğal geçişte gösterilir.
- Fotoğraf capture/select, Qwen inference, fal sonucu aktif okuma, tarot selection, el capture/analysis, chat typing/generation, rewarded, billing/restore/consent sırasında gösterilmez.

### Banner
- Telefon: tek anchored adaptive banner, yalnız güvenli ekranlarda.
- Tablet/BlueStacks: gerekirse tek side rail/kolon.
- Chat, analiz, capture, tarot selection, el analiz ekranında banner yok.
- CTA/nav/input'a yanlış tıklama doğuracak yakınlık yok.

### Premium
Premium aktifken Rewarded, timed interstitial, App Open, banner/native dahil **bütün reklam sistemi kapalıdır**.
- Yüklü reklam objeleri dispose edilir.
- Yeni reklam request'i gönderilmez.
- Reklam container/boş alanı kalmaz.

---

## 9. Billing / Google Play restore gate'i

`BILLING_RESTORE_SPEC.md` bu bölümün detaylı normatif kaynağıdır.

Tek ürün: **aylık otomatik yenilenen reklamsız Premium**. Lifetime/non-consumable Premium V1 kapsamında değildir.

MIZANGLOBAL yalnız read-only mimari referanstır. LP FAL'e taşınacak desen:
`purchaseStream → Google Play owned-entitlement sync/restore → doğrulanmış local snapshot → merkezi ad suppression`.

Zorunlu mimari:
- purchase listener satın alma/restore akışından önce hazır,
- purchase ve restore tek validation hattında,
- local Premium boolean tek başına source of truth değil,
- Play purchase token/verification data olmadan ücretli Premium yok,
- startup/app-resume/reconnect silent sync,
- manuel `Satın Alımları Geri Yükle`,
- duplicate callback idempotent,
- process-death recovery,
- Store fiyatı/para birimi Play metadata'sından,
- `Aboneliği Yönet` Google Play yönetim akışına bağlı.

State matrisi:
- active / renewed → Premium açık,
- pending initial purchase → Premium kapalı,
- grace period → Premium açık,
- cancelled fakat paid-through-end → dönem bitene kadar Premium açık,
- account hold → Premium kapalı,
- expired → Premium kapalı,
- revoked/refunded ve entitlement yok → Premium kapalı.

Test zorunlu:
- [ ] Yeni aylık purchase.
- [ ] Pending purchase.
- [ ] User cancel/error flow.
- [ ] Clean install + aynı Play hesabında aktif abonelik → otomatik restore.
- [ ] Reinstall/app-data-clear sonrası restore.
- [ ] Manuel restore.
- [ ] Zaten aktif Premium'da restore idempotent.
- [ ] App resume silent sync.
- [ ] Offline → online reconnect silent sync.
- [ ] Renewal.
- [ ] Grace period.
- [ ] Cancelled-but-paid-through-end.
- [ ] Account hold.
- [ ] Expiry.
- [ ] Revoke/refund entitlement removal.
- [ ] Process death purchase sırasında ve sonraki recovery.
- [ ] Duplicate purchase-stream callback.
- [ ] Invalid/empty verification data Premium üretmiyor.
- [ ] Subscription management link.
- [ ] Store fiyatı hard-code değil.
- [ ] Premium aktifleşince bütün yüklü reklamlar dispose.
- [ ] Premium aktifken sıfır ad request/container.

---

## 10. AI report / mağaza / privacy gate'i

- Uygulama içi AI output report/flag akışı.
- Kullanıcı başlatmadan report verisi gönderilmez.
- Ham kahve/el fotoğrafı report payload'a varsayılan eklenmez.
- Privacy Policy kalıcı HTTPS URL.
- Data Safety gerçek SDK davranışına göre.
- Qwen lisans/NOTICE.
- Üçüncü taraf asset lisansları.
- AdMob UMP gereken bölgelerde request öncesi.
- Development yalnız test reklam ID'leri.
- Release build'de doğrulanmış production AdMob ID'leri; test ID sızıntısı yok.

---

## 11. Ek QA matrisi

- [ ] Kahve multi-view fusion.
- [ ] Kahve confidence calibration.
- [ ] Kahve negatif görüntüler.
- [ ] El görünmeyen çizgi uydurmama.
- [ ] El farklı ten tonu/ışık/kamera çeşitliliği.
- [ ] El biometric/hassas trait jailbreak testi.
- [ ] Kesinlik/tavsiye safety regresyonu.
- [ ] Temp photo cleanup.
- [ ] Photo Picker permission.
- [ ] Auto Backup exclusion.
- [ ] `0/2 → 1/2 → 2/2` reward state.
- [ ] 90 saniye timed eligibility.
- [ ] Timed ad yalnız doğal geçiş.
- [ ] Telefon banner güvenli spacing.
- [ ] Tablet/BlueStacks side banner layout.
- [ ] Premium'da sıfır ad request/container.
- [ ] Billing clean-install restore.
- [ ] Billing pending/grace/account-hold/expiry.
- [ ] Billing process-death/reconnect recovery.
- [ ] Network isolation Kahve + El + Chat.
- [ ] Production sensitive logs kapalı.
- [ ] TalkBack/font scaling.

---

# BLOKLAYICI ÖZET

Aşağıdakilerden biri eksikse final yok:

1. Kahve + Tarot + El Falı tamam.
2. Kesin gelecek ve karar yönlendiren tavsiye yok.
3. El Falı biometric kimlik sistemi değil.
4. Kahve/El yalnız görünür görsel bulguyu yorumluyor.
5. Kahve/El confidence kalibre.
6. Qwen lokal inference.
7. Her free reward unlock 2 Rewarded.
8. Timed interstitial 90 sn eligibility + doğal geçiş.
9. Banner güvenli yerleşim.
10. Premium tamamen reklamsız ve entitlement Google Play restore/sync ile doğrulanıyor.
11. Clean install/reinstall/process-death restore testleri başarılı.
12. Fotoğraflar varsayılan geçici ve eğitim verisi değil.
13. Privacy/Data Safety/AI report/lisans tamam.
14. 4/6/8 GB + tablet/BlueStacks QA.
15. Signed APK/AAB clean test başarılı.

**Bu dosya `TODO.md` final fazının zorunlu girdisidir.**