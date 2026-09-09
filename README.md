# LP FAL

Android üzerinde yerel Qwen tabanlı **Kahve Falı + Tarot Falı + Rüya Tabiri + El Falı** uygulaması.

## V1 zorunlu dokümanları

Geliştirme ve final kontrolünde aşağıdaki dosyalar birlikte takip edilir:

1. [`SPECIFICATION.md`](./SPECIFICATION.md) — ürün, AI, güvenlik ve teknik ana şartname.
2. [`DREAM_INTERPRETATION_SPEC.md`](./DREAM_INTERPRETATION_SPEC.md) — **Rüya Tabiri için normatif V1 şartnamesi; Rüya Tabiri kapsamındaki eski çelişkili ifadeleri geçersiz kılar.**
3. [`COMPLIANCE_BY_DESIGN.md`](./COMPLIANCE_BY_DESIGN.md) — **uygulamanın tamamına uygulanan hukuka/politikalara uygunluk ve AI safety gate'leri.**
4. [`TODO.md`](./TODO.md) — bağımlılıklara göre sıralanmış ana faz listesi.
5. [`TODO_DREAM_COMPLIANCE.md`](./TODO_DREAM_COMPLIANCE.md) — **Rüya Tabiri fazı ve her faza uygulanan compliance görevleri; ana TODO ile birlikte zorunludur.**
6. [`V1_RELEASE_GATES.md`](./V1_RELEASE_GATES.md) — bloklayıcı ana release şartları; `COMPLIANCE_BY_DESIGN.md` ek gate'leri de final için zorunludur.
7. [`MONETIZATION_V1.md`](./MONETIZATION_V1.md) — Rewarded, 90 saniye timed interstitial, banner ve Premium reklam kuralları.
8. [`BILLING_RESTORE_SPEC.md`](./BILLING_RESTORE_SPEC.md) — Google Play satın alma/restore davranışı.
9. [`PRODUCT_MODEL_V1.md`](./PRODUCT_MODEL_V1.md) — **Premium ürün modeli ve isim standardında ana kaynak**.
10. [`FUNCTIONAL_UI_GATES.md`](./FUNCTIONAL_UI_GATES.md) — mockup'ların gerçek Flutter UI'a dönüşüm kabul şartları.
11. [`UI_ASSET_MANIFEST.md`](./UI_ASSET_MANIFEST.md) — repo içindeki üretilmiş UI görsellerinin dosya/SHA envanteri ve görsel-copy gate'i.

## V1 kapsamı

- Kahve Falı
- Tarot Falı
- **Rüya Tabiri**
- El Falı
- Fal/rüya sonucu ve aktif yoruma bağlı sohbet
- Geçmiş Fallar / Rüyalar
- Yerel Qwen inference
- AdMob monetizasyonu
- **LP FAL Premium: tek seferlik Google Play satın alımı, abonelik yok**

### Kapsam önceliği
`SPECIFICATION.md` içindeki eski `Rüya tabiri V1'e eklenmeyecek` ifadesi artık geçersizdir. Rüya Tabiri konusunda `DREAM_INTERPRETATION_SPEC.md`; uygulama genelinde hukuka/politikalara uygunluk konusunda `COMPLIANCE_BY_DESIGN.md` önceliklidir.

Premium kullanıcı arayüzünde yalnız **Premium** adı kullanılır. Eski plan etiketi veya süre/abonelik çağrıştıran paket adı kullanıcıya gösterilmez.

Satın alma ekranı açıklaması: **Tek seferlik satın alım · Abonelik değildir.**

## Uygulama genelinde yorum sınırı

Kahve, Tarot, Rüya Tabiri, El Falı ve bunlara bağlı sohbetler **eğlence ve kişisel yorum** çerçevesindedir. Hiçbir modül:
- kesin gelecek iddiası,
- tıbbi/psikolojik teşhis,
- hukuki/finansal/dini profesyonel danışmanlık,
- kullanıcı adına önemli hayat kararı,
- doğaüstü iddiayı gerçek dünya kanıtı olarak doğrulama
üretemez.

Safety yalnız prompt'a bırakılmaz; structured grounding + application-level compliance filter + in-app AI report/flag birlikte uygulanır.

V1 tamamlanmadan kapsam dışı yeni fal türü veya yeni ürün modülü eklenmez.