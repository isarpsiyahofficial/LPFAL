# LP FAL — Premium Ürün Modeli ve İsim Standardı

**Durum:** ZORUNLU / normatif  
**Karar tarihi:** 2026-09-09  
**Öncelik:** Premium ürün modeli, satın alma ve kullanıcıya görünen Premium/PRO adlandırması konusunda bu dosya diğer eski dokümanlardaki çelişkili ifadeleri geçersiz kılar.

## 1. Tek ücretli ürün

LP FAL V1'de tek ücretli ürün **LP FAL Premium**'dur.

- Aylık/yıllık/otomatik yenilenen abonelik YOKTUR.
- Tek seferlik Google Play satın alımı vardır.
- Satın alım başarılı ve geçerli Google Play hakkıyla doğrulandığında Premium erişimi kalıcıdır.
- Tüketiciye görünen paket adı `Ömür Boyu`, `Lifetime`, `Permanent`, `PRO` veya benzeri teknik/marketing varyasyonlar olmayacaktır.
- Ekranda ürün adı yalnız **LP FAL Premium** olarak gösterilir.
- Açıklama: **Tek seferlik satın alım · Abonelik değildir.**
- Satın alma CTA'sı: **Premium'a Geç** veya **Premium'u Aç**.
- Restore CTA'sı: **Satın Alımı Geri Yükle**.

## 2. Google Play restore

- Premium entitlement'ın çevrimiçi kaynağı Google Play satın alma kaydıdır.
- Uygulama açılışında, resume'da ve internet tekrar geldiğinde owned purchase durumu sessizce senkronize edilir.
- Aynı Google Play hesabıyla clean install/yeni cihaz sonrasında kullanıcı ayrıca ödeme yapmadan satın alımı geri alabilmelidir.
- Manuel `Satın Alımı Geri Yükle` aksiyonu ayrıca bulunur.
- Purchase ve restore aynı entitlement doğrulama hattından geçer.
- Yerel cache tek başına ücretli Premium üretmez.
- Duplicate callback/idempotency korunur.
- Process death sonrasında sonraki açılışta owned purchase sync ile hak tekrar bulunur.

## 3. Premium reklam davranışı

Premium aktifken:
- Rewarded yok.
- Timed interstitial yok.
- App Open yok.
- Banner/native yok.
- Yeni reklam request'i yapılmaz.
- Önceden yüklenmiş reklam nesneleri dispose edilir.
- Boş reklam container'ı bırakılmaz.
- Kahve, Tarot, El Falı ve fal sohbeti reklam kapısına takılmaz.

Premium yoksa `MONETIZATION_V1.md` içindeki Free reklam kuralları uygulanır.

## 4. İsim standardı — PRO yasak

LP FAL kullanıcı arayüzünde Premium özelliği için tek marka kelimesi **Premium**'dur.

Yasak/legacy kullanıcı metinleri:
- PRO
- Pro
- LP FAL PRO
- Pro'ya Geç
- Pro satın al
- Ömür Boyu Premium
- Lifetime Premium

Doğru örnekler:
- Premium
- LP FAL Premium
- Premium'a Geç
- Premium Aktif
- Satın Alımı Geri Yükle
- Tek seferlik satın alım · Abonelik değildir.

`Lefferion Prime` marka adındaki `Prime` bu kurala tabi değildir; bu bir marka adıdır, Premium ürün etiketi değildir.

## 5. Görsel referans kuralı

- `design_refs/ui/` altındaki eski JPG mockup'larda `PRO/Pro` görülmesi durumunda bu metin **geçersiz legacy copy** kabul edilir.
- Uygulama runtime'ında mockup screenshot'ı kullanılmayacaktır; gerçek Flutter widget metni daima `Premium` olacaktır.
- Yeni/yenilenen görsel referanslarda `PRO/Pro` kullanılamaz.
- `assets/ui/` içindeki dekoratif görsellerde ürün plan adı/metni gömülmemesi tercih edilir; plan adı gerekiyorsa yalnız `Premium` kullanılır.

## 6. Release gate

Aşağıdakilerden biri varsa release bloklanır:
- subscription/aylık/yıllık Premium ürünü,
- Premium için `PRO/Pro` kullanıcı metni,
- satın alma sonrası entitlement'ın yalnız local boolean ile verilmesi,
- reinstall/new-device restore'un çalışmaması,
- Premium aktifken herhangi bir reklam request/container/gösterimi,
- satın alma ekranında `Ömür Boyu` veya `Lifetime` paket adı.

**Bu dosya ürün modeli ve Premium isimlendirmesinde tek kaynak kabul edilir.**