# LP FAL — V1 Yapılacaklar Listesi

Bu liste `SPECIFICATION.md`, `PRODUCT_MODEL_V1.md`, `V1_RELEASE_GATES.md`, `MONETIZATION_V1.md`, `BILLING_RESTORE_SPEC.md` ve `FUNCTIONAL_UI_GATES.md` ile birlikte uygulanır.

Premium ürün tipi ve kullanıcıya görünen adlandırmada `PRODUCT_MODEL_V1.md` ana kaynaktır.

---

## FAZ 0 — Repo, kimlik ve çalışma kuralları
- [ ] Flutter stable proje iskeletini oluştur.
- [ ] Android `applicationId` değerini sabitle (`com.lefferionprime.lpfal` önerilen).
- [ ] min/compile/target SDK değerlerini güncel Play + AI runtime gereksinimine göre belirle.
- [ ] `.gitignore`, lint, `analysis_options.yaml`.
- [ ] Feature-first klasör yapısı.
- [ ] Secrets/release signing ayrımı.
- [ ] CI: analyze + test + Android build.
- [ ] Normatif dokümanları repo kökünde kaynak kabul et.

**Bitiş:** temiz repo + çalışan boş debug APK.

---

## FAZ 1 — Marka + isim standardı
- [ ] MIZANGLOBAL mevcut Lefferion Prime logosunu LP FAL'a aktar.
- [ ] Görünen ad her yerde `LP FAL`.
- [ ] Launcher + splash.
- [ ] Premium kullanıcı metni yalnız `Premium / LP FAL Premium`.
- [ ] `PRO/Pro/LP FAL PRO` runtime copy'de yasak.
- [ ] `Ömür Boyu Premium`, `Lifetime Premium`, `Aylık Premium` plan adı yasak.
- [ ] Satın alma açıklaması `Tek seferlik satın alım · Abonelik değildir.`
- [ ] `Lefferion Prime` marka adı bu Premium copy kontrolünden ayrı tutulur.

**Bitiş:** marka ve Premium terminolojisi tek standarda bağlı.

---

## FAZ 2 — Görsel asset ve design system
- [ ] Açık/krem renk tokenları.
- [ ] Tipografi/spacing/radius/shadow/button/card standardı.
- [ ] Repo içindeki `assets/ui/` dosyalarını inventory ile doğrula.
- [ ] `design_refs/ui/` mockup'larını inventory ile doğrula.
- [ ] Kahve/Tarot/El statik asset lisanslarını kayıt altına al.
- [ ] Tarot 78 kart setini hazırla.
- [ ] Görselleri optimize et.
- [ ] Yeni görsel referansta `PRO/Pro` kullanma.
- [ ] Kullanıcıya görünen buton/başlık/metni screenshot içine gömme; Flutter widget üret.

**Bitiş:** bütün V1 statik görseller repo içinde, lisanslı ve isim standardıyla uyumlu.

---

## FAZ 3 — Navigation + Dashboard UI
- [ ] Header: logo + LP FAL + profil/ayar.
- [ ] Kahve Falı hero kartı.
- [ ] Tarot Falı kartı.
- [ ] El Falı kartı.
- [ ] Son Falın / Sohbete Dön conditional kartı.
- [ ] Bottom nav: Ana Sayfa / Fallarım / Premium / Profil.
- [ ] Dar telefon ve tablet responsive grid.
- [ ] 360dp + tablet + BlueStacks overflow testi.

**Bitiş:** V1 modülleri gerçek navigation ile erişilebilir.

---

## FAZ 4 — Yerel Qwen runtime PoC
- [ ] Resmi Qwen3.5-0.8B revision/lisans sabitle.
- [ ] Android inference runtime seç.
- [ ] Kahve + El için multimodal desteği gerçek cihazda doğrula.
- [ ] Q4 ve alternatif quant benchmark.
- [ ] 4/6/8 GB load + RAM + token/s + OOM testleri.
- [ ] Flutter ↔ native/FFI inference katmanını ayır.
- [ ] UI main thread bloklanmasın.
- [ ] Missing/corrupt/unsupported state.

**Bitiş:** internetsiz Türkçe text + test image inference telefonda çalışıyor.

---

## FAZ 5 — Debug/local model kurulumu
- [ ] Model Git history'ye commit edilmez.
- [ ] Debug local model path.
- [ ] ADB install script.
- [ ] App-private model storage.
- [ ] Model/projector SHA-256.

**Bitiş:** Play olmadan clean phone model + APK testi.

---

## FAZ 6 — Kamera / Photo Picker / privacy pipeline
- [ ] System Photo Picker.
- [ ] Kamera capture.
- [ ] Gereksiz broad storage permission yok.
- [ ] Kamera izni yalnız capture başlatınca.
- [ ] EXIF/orientation normalize.
- [ ] App-private temp file.
- [ ] Blur/exposure/resolution ortak kalite yardımcıları.
- [ ] Temp photo cleanup.
- [ ] Production log redaction.
- [ ] Auto Backup exclusion.
- [ ] Fotoğraf training'e varsayılan gitmez.

**Bitiş:** Kahve/El ortak güvenli media abstraction.

---

## FAZ 7 — Kahve preprocessing
- [ ] 2–3 fotoğraf; 3 önerisi.
- [ ] Fincan/fincan içi görünürlük.
- [ ] Wrong-image kontrolü.
- [ ] Crop/normalize.
- [ ] Ağız/orta/dip/kulp region extraction.
- [ ] Kalitesiz fotoğrafta AI öncesi retry.
- [ ] Çekim yönergesi.

---

## FAZ 8 — Kahve AI pipeline
- [ ] İlk Qwen aşaması yalnız structured visual analysis.
- [ ] region/shape/rawConfidence/view modeli.
- [ ] Multi-photo merged analysis.
- [ ] Duplicate dedup.
- [ ] Çelişkili bulgu uncertain/eleme.
- [ ] Self-confidence kör kullanılmaz.
- [ ] İkinci Qwen aşaması yalnız merged analysis üzerinden fal metni.
- [ ] Genel/Aşk/İş-Para/Yol-Değişim.
- [ ] Safety filter.

---

## FAZ 9 — Kahve doğruluk QA
- [ ] En az 100 gerçek fincan fotoğrafı/grubu.
- [ ] Human ground truth.
- [ ] Same-cup multi-view örnekleri.
- [ ] Negative/no-symbol örnekler.
- [ ] Zor ışık/bulanık/yanıltıcı pattern.
- [ ] Confidence calibration.
- [ ] High-confidence precision hedef ≥ %80.
- [ ] High-confidence false-positive hedef ≤ %15.

---

## FAZ 10 — Tarot engine + UI
- [ ] 78 kart metadata + asset mapping.
- [ ] Türkçe kart adları.
- [ ] Secure RNG.
- [ ] Duplicate kart engeli.
- [ ] Upright/reversed merkezi config.
- [ ] 1 / 3 / 5 kart açılımı.
- [ ] Kullanıcı sorusu opsiyonel.
- [ ] Shuffle + closed-card selection UX.
- [ ] Qwen'e structured card metadata.
- [ ] Qwen listede olmayan kart uyduramaz.
- [ ] 78 kart integrity testi.

---

## FAZ 11 — El Falı preprocessing
- [ ] 1–2 avuç fotoğrafı.
- [ ] Doğru çekim rehberi.
- [ ] El/avuç varlık kontrolü.
- [ ] Blur/resolution/exposure/occlusion.
- [ ] Avuç crop/normalize.
- [ ] Wrong-image retry.
- [ ] Ham fotoğraf inference sonrası default silinir.
- [ ] Fingerprint/biometric template yok.

---

## FAZ 12 — El Falı AI pipeline
- [ ] Structured visible-palm analysis.
- [ ] Kalp/baş/yaşam/kader yalnız görünüyorsa.
- [ ] not_visible/uncertain state.
- [ ] Confidence validation.
- [ ] Structured analysis → sembolik yorum.
- [ ] Sağlık/ölüm/hamilelik/hassas trait engeli.
- [ ] Gereksiz yaş/cinsiyet tahmini yok.
- [ ] Safety filter.

---

## FAZ 13 — El Falı doğruluk / bias QA
- [ ] En az 100 gerçek avuç görüntüsü/grubu.
- [ ] Visible-line ground truth.
- [ ] Farklı ten tonu/ışık/kamera/pozisyon.
- [ ] Negative/not-visible örnekler.
- [ ] False-positive calibration.
- [ ] Sensitive trait jailbreak testi.

---

## FAZ 14 — Ortak fal sonucu + Fal Sohbeti
- [ ] Ortak result shell.
- [ ] Zorunlu entertainment disclaimer.
- [ ] Kahve/Tarot/El context bağla.
- [ ] Her fal için unique local conversation.
- [ ] Fal dışı scope rejection.
- [ ] Prompt injection guard.
- [ ] Local context summary.
- [ ] Loading/error/retry/cancel.
- [ ] Free: ilk takip sorusu ücretsiz.
- [ ] Free: sonraki her 3 user-message pack = 2 Rewarded.
- [ ] Premium: reklam kapısı yok.

---

## FAZ 15 — Geçmiş Fallar + local storage
- [ ] Local DB/storage seç.
- [ ] Kahve/Tarot/El kayıt tipleri.
- [ ] Structured analysis + final text.
- [ ] Chat history fala bağlı.
- [ ] Delete flow.
- [ ] Raw fotoğraf default kalıcı saklanmaz.

---

## FAZ 16 — Free reklam sistemi
- [ ] AdMob + UMP.
- [ ] Test build yalnız test IDs.
- [ ] Rewarded unlock transaction `0/2→1/2→2/2`.
- [ ] İkinci rewarded auto-open olmaz.
- [ ] Single rewarded unlock vermez.
- [ ] 90 sn foreground active eligibility.
- [ ] Timed ad yalnız natural/safe transition.
- [ ] Phone anchored adaptive banner.
- [ ] Tablet/BlueStacks single side rail gerekirse.
- [ ] Chat/capture/analysis/tarot-selection/billing ekranında banner yok.
- [ ] App Open collision guard.

**Bitiş:** Free reklam akışları merkezi, testli ve policy-safe.

---

## FAZ 17 — LP FAL Premium: tek seferlik satın alma
- [ ] Tek Google Play ürünü: **LP FAL Premium**.
- [ ] Product type kalıcı/non-consumable entitlement.
- [ ] Subscription/base-plan kodu yok.
- [ ] Fiyat Google Play metadata'sından.
- [ ] UI: `Premium'a Geç` / `Premium'u Aç`.
- [ ] Açıklama: `Tek seferlik satın alım · Abonelik değildir.`
- [ ] `PRO/Pro/Ömür Boyu/Lifetime/Aylık Premium` UI copy yok.
- [ ] Purchase stream merkezi.
- [ ] Pending Premium vermez.
- [ ] Cancel/error Premium vermez.
- [ ] Valid purchased/restored Premium verir.
- [ ] Owned purchase query/silent restore.
- [ ] Clean-install restore.
- [ ] New-device restore.
- [ ] App resume sync.
- [ ] İnternet geri gelişi sync.
- [ ] Process-death recovery.
- [ ] Duplicate callback idempotency.
- [ ] Invalid/empty proof Premium vermez.
- [ ] Manual `Satın Alımı Geri Yükle`.
- [ ] Premium active → Rewarded/interstitial/App Open/banner/native kapalı.
- [ ] Loaded ads dispose.
- [ ] Empty ad container yok.

**Bitiş:** tek seferlik Premium purchase/restore güvenli ve tamamen reklamsız.

---

## FAZ 18 — Privacy / Play / lisans
- [ ] Privacy Policy HTTPS.
- [ ] Data Safety gerçek SDK davranışına göre.
- [ ] AI report kullanıcı tarafından başlatılır.
- [ ] Raw photo report payload'a default eklenmez.
- [ ] Qwen license/NOTICE.
- [ ] Third-party visual asset licenses.
- [ ] AdMob UMP gereken bölgelerde request öncesi.

---

## FAZ 19 — Responsive / accessibility / performance QA
- [ ] 360dp telefon.
- [ ] Büyük telefon.
- [ ] Tablet.
- [ ] BlueStacks.
- [ ] Safe area/notch/gesture nav.
- [ ] Font scaling.
- [ ] TalkBack.
- [ ] Critical touch target >=48dp.
- [ ] 4/6/8 GB model performance.
- [ ] Main thread freeze yok.

---

## FAZ 20 — Network isolation / regression / release
- [ ] AI photo/prompt/chat network'e çıkmıyor.
- [ ] Production sensitive logs kapalı.
- [ ] Coffee/Tarot/Palm/Chat safety regression.
- [ ] Premium text scan: `PRO/Pro/Ömür Boyu/Lifetime/Aylık Premium` yok.
- [ ] Subscription dependency/config yok.
- [ ] Asset inventory eksiksiz.
- [ ] Signed APK clean install.
- [ ] Signed AAB build.
- [ ] Purchase + restore Play test.
- [ ] Premium active → zero ad request/container.

**Bitiş:** tüm release gate'leri geçti, final APK/AAB üretildi.

---

# BLOKLAYICI MUST-HAVE

Aşağıdakilerden biri eksikse V1 final değildir:
1. Kahve + Tarot + El Falı gerçek işlevli.
2. Yerel Qwen inference.
3. Kahve/El structured visual pipeline + doğruluk QA.
4. Tarot 78 kart integrity.
5. Fala bağlı gerçek chat.
6. Free 2-Rewarded unlock sistemi.
7. 90 saniye timed eligibility + safe transition.
8. Premium tek seferlik Google Play satın alımı; abonelik yok.
9. Reinstall/new-device/process-death restore.
10. Premium aktifken sıfır reklam request/container.
11. Premium ürün copy'sinde `PRO/Pro/Ömür Boyu/Lifetime/Aylık Premium` yok.
12. Privacy/Data Safety/licenses.
13. 4/6/8 GB + tablet/BlueStacks QA.
14. Signed APK/AAB clean test.

Checkbox tek başına yeterli değildir; ilgili fazın bitiş kriteri gerçek testle doğrulanmalıdır.