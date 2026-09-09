# LP FAL — V1 Ek Release Gates

**Durum:** ZORUNLU / normatif  
**Bağlı dosyalar:** `SPECIFICATION.md`, `DREAM_INTERPRETATION_SPEC.md`, `COMPLIANCE_BY_DESIGN.md`, `TODO.md`, `TODO_DREAM_COMPLIANCE.md`, `MONETIZATION_V1.md`, `BILLING_RESTORE_SPEC.md`, `PRODUCT_MODEL_V1.md`, `FUNCTIONAL_UI_GATES.md`

Premium ürün modeli ve kullanıcıya görünen isim standardında `PRODUCT_MODEL_V1.md`; Rüya Tabiri konusunda `DREAM_INTERPRETATION_SPEC.md`; global compliance konusunda `COMPLIANCE_BY_DESIGN.md` ana kaynaklardır.

Bu dosyadaki maddeler V1 final kontrolünün zorunlu parçasıdır.

---

## 1. Kapsam kilidi

V1 ana yorum modülleri yalnız:
- Kahve Falı,
- Tarot Falı,
- **Rüya Tabiri**,
- El Falı.

V1'e burç/astroloji, numeroloji, sosyal özellik, coin/kredi veya genel AI asistanı eklenmez.

V1 kullanıcı arayüzü ve yorum üretim dili Türkçe'dir; kod i18n'e hazır olabilir.

---

## 2. Ortak güvenli yorum release gate'i

Kahve, Tarot, Rüya, El ve bunlara bağlı sohbetler için:
- kesin gelecek iddiası yok,
- kullanıcıya önemli hayat kararı aldıran emir/tavsiye yok,
- sağlık teşhisi yok,
- psikolojik/psikiyatrik tanı yok,
- ölüm/yaşam süresi tahmini yok,
- hamilelik/doğurganlık kesinliği yok,
- hukuki sonuç garantisi yok,
- yatırım/şans/finansal kazanç garantisi yok,
- dini otorite/fetva/ilahi mesaj kesinliği yok,
- büyü/cin/lanet/nazar/paranoya gerçek olgu olarak doğrulanmaz.

Sonuç dili sembolik ve koşullu olmalıdır: `çağrıştırabilir`, `geleneksel yorumlarda`, `sembolik olarak`.

Her fal/rüya sonucu görünür entertainment/professional-advice disclaimer içerir.
Safety yalnız system prompt'a bırakılmaz; uygulama seviyesinde output compliance kontrolü bulunur.

Her AI sonucu/chat mesajı uygulamadan çıkmadan report/flag edilebilir.

---

## 3. Fotoğraf, rüya metni ve veri gizliliği

Kahve + El için:
- System Photo Picker/scoped yaklaşım tercih edilir.
- Broad storage permission mümkün olduğunca yok.
- Kamera izni yalnız kullanıcı capture başlatınca.
- Fotoğraf app-private geçici alanda.
- Ham görüntüler varsayılan inference sonrası silinir.
- Auto Backup ile ham fotoğraf/model/AI cache izinsiz buluta gitmez.

Rüya + tüm chat için:
- ham rüya/chat metni analytics'e yazılmaz,
- production loglarına yazılmaz,
- reklam segmentasyonuna dönüştürülmez,
- inference için harici AI servisine gönderilmez,
- report payload'a tamamı varsayılan otomatik eklenmez.

Tüm modüller için:
- kullanıcı verileri varsayılan training verisine dönüşmez,
- KVKK veri envanteri/aydınlatma/privacy gerçek veri akışıyla eşleşir.

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

## 6. Rüya Tabiri groundedness / safety gate'i

Zorunlu:
- gerçek multiline rüya input,
- structured `dream_extract`,
- yalnız kullanıcının anlattığı sahne/sembol/duygu/roller,
- rüyada olmayan ayrıntıyı uydurmama,
- `dream_interpret` yalnız structured veriden üretim,
- belirsiz öğe için uncertain davranışı,
- sonuçta tavsiye/öneri/karar bölümü olmaması,
- in-app report/flag,
- active-dream chat context,
- local history/persistence/delete.

Kesin yasak:
- sağlık/psikoloji teşhisi,
- hamilelik/ölüm/gelecek kesinliği,
- hukuki/finansal yönlendirme,
- dini otorite/ilahi kesinlik,
- büyü/cin/lanet/paranoya doğrulama,
- üçüncü kişi hassas trait çıkarımı.

QA:
- en az 100 Türkçe Dream senaryosu,
- `kesin söyle`, `ne yapmalıyım`, prompt injection ve high-risk senaryolar dahil.

---

## 7. El Falı özel güvenlik ve doğruluk gate'i

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

## 8. AI sürümleme ve on-device gate'i

Her release kaydı:
- Qwen model/revision,
- quantization,
- model SHA-256,
- vision projector/encoder SHA-256 varsa,
- runtime sürümü,
- coffee/tarot/dream_extract/dream_interpret/dream_chat/palm/chat/safety/compliance prompt sürümleri,
- generation parametreleri.

Model veya prompt değişince regresyon tekrar çalışır.
AI inference için fotoğraf/rüya metni/prompt/chat sunucuya gönderilmez. Cloudflare/harici inference yok.
Debug model Play olmadan local kurulabilir. Release model base APK içine gömülmez; release tarihindeki Play on-device/asset delivery yöntemi yeniden doğrulanır.

---

## 9. Monetizasyon gate'i

`MONETIZATION_V1.md` bu konuda ana kaynaktır.

### Rewarded
Ücretsiz kullanıcıda:
- Kahve full result = 2 Rewarded,
- Tarot full result = 2 Rewarded,
- **Rüya full result = 2 Rewarded,**
- El Falı full result = 2 Rewarded,
- gated fal/rüya chat pack = 2 Rewarded.

- UI `0/2 → 1/2 → 2/2`.
- Her reklam ayrı kullanıcı opt-in.
- İlk reklam tek başına entitlement vermez.
- İkinci başarılı `reward earned` callback gelmeden unlock yok.
- Ad failure ücretsiz entitlement üretmez.

### Timed interstitial
V1 sabiti: `timedInterstitialEligibilitySeconds = 90`

- Yalnız foreground aktif kullanım sayılır.
- 90 sn dolması sadece eligibility oluşturur.
- Reklam ancak sonraki güvenli/doğal geçişte gösterilir.
- Fotoğraf capture/select, rüya yazma, Qwen inference, fal/rüya sonucu aktif okuma, tarot selection, el capture/analysis, chat typing/generation, rewarded, billing/consent sırasında gösterilmez.

### Banner
- Telefon: tek anchored adaptive banner, yalnız güvenli ekranlarda.
- Tablet/BlueStacks: gerekirse tek side rail/kolon.
- Rüya input/result/chat, genel chat, analiz, capture, tarot selection, el analiz ekranında banner yok.
- CTA/nav/input'a yanlış tıklama doğuracak yakınlık yok.
- Fal/rüya içeriği hassas reklam profiline dönüştürülmez.

### Premium
Premium aktifken Rewarded, timed interstitial, App Open, banner/native dahil **bütün reklam sistemi kapalıdır**.

---

## 10. Premium satın alma / restore gate'i

Tek ürün: **LP FAL Premium**.

- Tek seferlik Google Play satın alımıdır.
- Subscription/aylık/yıllık/otomatik yenileme yoktur.
- Kullanıcıya `Ömür Boyu` veya `Lifetime` plan adı gösterilmez.
- Açıklama: `Tek seferlik satın alım · Abonelik değildir.`
- Fiyat Google Play metadata'sından gelir; hard-code değildir.

Test zorunlu:
- purchase success,
- pending,
- cancel flow,
- purchase error,
- owned-purchase query/restore,
- clean install restore,
- new-device restore,
- app resume sync,
- internet-return sync,
- process-death recovery,
- duplicate callback/idempotency,
- invalid/empty purchase proof,
- wrong product ID,
- acknowledgement/complete path,
- manual `Satın Alımı Geri Yükle`.

Premium aktif olduğunda ad request/container dahil tüm reklam yüzeyi sıfır olmalıdır.

---

## 11. Premium isim standardı gate'i

Kullanıcıya görünen Premium özelliğinde yalnız:
- `Premium`
- `LP FAL Premium`
kullanılır.

Release'i bloklayan legacy copy:
- `PRO`
- `Pro`
- `LP FAL PRO`
- `Ömür Boyu Premium`
- `Lifetime Premium`
- `Aylık Premium`

`Lefferion Prime` marka adı bu kontrolden muaftır.

`design_refs/ui/` içindeki eski JPG'lerde legacy copy varsa runtime copy kaynağı olamaz; yeni/yenilenen görsel referansta yalnız `Premium` kullanılmalıdır.

---

## 12. Compliance / AI report / mağaza / privacy gate'i

`COMPLIANCE_BY_DESIGN.md` eksiksiz uygulanır.

Zorunlu:
- Uygulama içi AI output report/flag akışı.
- Kullanıcı başlatmadan report verisi gönderilmez.
- Ham kahve/el fotoğrafı ve ham rüya/chat report payload'a varsayılan eklenmez.
- Privacy Policy kalıcı HTTPS URL.
- KVKK aydınlatma/veri envanteri gerçek veri akışıyla eşleşir.
- Data Safety gerçek SDK davranışına göre.
- Store listing/screenshot yanıltıcı vaat içermez.
- Qwen lisans/NOTICE.
- Üçüncü taraf asset lisansları.
- AdMob UMP gereken bölgelerde request öncesi.
- Development yalnız test reklam ID'leri.
- Dark pattern yok.
- Health declaration güncel Play gereksinimine göre doğru doldurulur.
- Release tarihinde Google Play/KVKK değişiklik kontrolü yapılır.

---

## 13. Ek QA matrisi

- [ ] Kahve multi-view fusion.
- [ ] Kahve confidence calibration.
- [ ] Kahve negatif görüntüler.
- [ ] Tarot 78 kart integrity.
- [ ] Rüya structured groundedness.
- [ ] Rüya inputunda olmayan ayrıntıyı uydurmama.
- [ ] Rüya `kesin söyle` / `ne yapmalıyım` / sağlık / hukuk / finans / din / doğaüstü regresyonu.
- [ ] Rüya self-harm/violence high-risk transition.
- [ ] El görünmeyen çizgi uydurmama.
- [ ] El farklı ten tonu/ışık/kamera çeşitliliği.
- [ ] El biometric/hassas trait jailbreak testi.
- [ ] Dört modül kesinlik/tavsiye safety regresyonu.
- [ ] AI in-app report/flag.
- [ ] Temp photo cleanup.
- [ ] Photo Picker permission.
- [ ] Auto Backup exclusion.
- [ ] Rüya/chat text production log/analytics sızıntısı yok.
- [ ] `0/2 → 1/2 → 2/2` reward state dört modülde.
- [ ] 90 saniye timed eligibility.
- [ ] Timed ad yalnız doğal geçiş.
- [ ] Telefon banner güvenli spacing.
- [ ] Tablet/BlueStacks side banner layout.
- [ ] Premium'da sıfır ad request/container.
- [ ] Tek seferlik Premium purchase + reinstall/new-device restore.
- [ ] Subscription ürünü yok.
- [ ] Premium UI copy'de `PRO/Pro` yok.
- [ ] Network isolation Kahve + Rüya + El + Chat.
- [ ] Production sensitive logs kapalı.
- [ ] TalkBack/font scaling.
- [ ] Store listing misleading-claim kontrolü.
- [ ] Privacy/Data Safety gerçek SDK envanteriyle eşleşiyor.

---

# BLOKLAYICI ÖZET

Aşağıdakilerden biri eksikse final yok:

1. Kahve + Tarot + **Rüya Tabiri** + El Falı tamam.
2. Kesin gelecek ve karar yönlendiren tavsiye yok.
3. Sağlık/psikoloji/hukuk/finans/din alanında otorite/teşhis/danışmanlık yok.
4. Rüya kullanıcı inputunda olmayan ayrıntıyı uydurmuyor.
5. Rüya doğaüstü iddiayı gerçek kanıt olarak doğrulamıyor.
6. El Falı biometric kimlik sistemi değil.
7. Kahve/El yalnız görünür görsel bulguyu yorumluyor.
8. Kahve/El confidence kalibre.
9. Qwen lokal inference.
10. Her free reward unlock 2 Rewarded.
11. Timed interstitial 90 sn eligibility + doğal geçiş.
12. Banner güvenli yerleşim.
13. Premium tamamen reklamsız.
14. Premium tek seferlik Google Play satın alımı; abonelik yok.
15. Restore clean-install/new-device/process-death testli.
16. Premium kullanıcı metinlerinde `PRO/Pro/Ömür Boyu/Lifetime` yok.
17. Fotoğraflar varsayılan geçici ve eğitim verisi değil.
18. Rüya/chat hassas metni log/analytics/ads segmentine gitmiyor.
19. AI report/flag uygulama içinde çalışıyor.
20. KVKK/Privacy/Data Safety/mağaza metinleri gerçek davranışla uyumlu.
21. `COMPLIANCE_BY_DESIGN.md` red-team gate'leri geçti.
22. 4/6/8 GB + tablet/BlueStacks QA.
23. Signed APK/AAB clean test başarılı.

**Bu dosya `TODO.md` + `TODO_DREAM_COMPLIANCE.md` final fazının zorunlu girdisidir.**