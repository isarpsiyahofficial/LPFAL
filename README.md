# LP FAL

Android üzerinde yerel Qwen tabanlı **Kahve Falı + Tarot Falı + El Falı** uygulaması.

## V1 zorunlu dokümanları

Geliştirme ve final kontrolünde aşağıdaki dosyalar birlikte takip edilir:

1. [`SPECIFICATION.md`](./SPECIFICATION.md) — ürün, AI, güvenlik ve teknik şartname.
2. [`TODO.md`](./TODO.md) — bağımlılıklara göre sıralanmış faz bazlı yapılacaklar listesi.
3. [`V1_RELEASE_GATES.md`](./V1_RELEASE_GATES.md) — bloklayıcı release şartları.
4. [`MONETIZATION_V1.md`](./MONETIZATION_V1.md) — 2 Rewarded, 90 saniye timed interstitial, banner ve Premium reklam kuralları.
5. [`BILLING_RESTORE_SPEC.md`](./BILLING_RESTORE_SPEC.md) — Google Play satın alma/restore davranışı.
6. [`PRODUCT_MODEL_V1.md`](./PRODUCT_MODEL_V1.md) — **Premium ürün modeli ve isim standardında ana kaynak**.
7. [`FUNCTIONAL_UI_GATES.md`](./FUNCTIONAL_UI_GATES.md) — mockup'ların gerçek Flutter UI'a dönüşüm kabul şartları.
8. [`UI_ASSET_MANIFEST.md`](./UI_ASSET_MANIFEST.md) — repo içindeki üretilmiş UI görsellerinin dosya/SHA envanteri ve görsel-copy gate'i.

## V1 kapsamı

- Kahve Falı
- Tarot Falı
- El Falı
- Fal sonucu ve fala bağlı sohbet
- Geçmiş Fallar
- Yerel Qwen inference
- AdMob monetizasyonu
- **LP FAL Premium: tek seferlik Google Play satın alımı, abonelik yok**

Premium kullanıcı arayüzünde yalnız **Premium** adı kullanılır. Eski plan etiketi veya süre/abonelik çağrıştıran paket adı kullanıcıya gösterilmez.

Satın alma ekranı açıklaması: **Tek seferlik satın alım · Abonelik değildir.**

Fal çıktıları eğlence/kişisel yorum amaçlıdır. Kesin gelecek iddiası, sağlık/hukuk/finans teşhisi veya kullanıcıya önemli hayat kararı aldıran yönlendirici tavsiye V1 güvenlik kurallarına aykırıdır.

V1 tamamlanmadan kapsam dışı yeni fal türü veya yeni ürün modülü eklenmez.