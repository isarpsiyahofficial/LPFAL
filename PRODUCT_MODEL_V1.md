# LP FAL — Premium Ürün Modeli ve İsim Standardı

**Durum:** ZORUNLU / normatif  
**Karar tarihi:** 2026-09-10  
**Öncelik:** Premium ürün modeli, satın alma ve kullanıcıya görünen Premium adlandırması konusunda bu dosya diğer eski dokümanlardaki çelişkili ifadeleri geçersiz kılar.

## 1. Tek ücretli ürün

LP FAL V1'de tek ücretli ürün **LP FAL Premium**'dur.

- Aylık/yıllık/otomatik yenilenen abonelik YOKTUR.
- Tek seferlik Google Play satın alımı vardır.
- Satın alım başarılı ve geçerli Google Play hakkıyla doğrulandığında Premium erişimi kalıcıdır.
- Tüketiciye görünen paket adı `Ömür Boyu`, `Lifetime`, `Permanent`, `PRO` veya benzeri teknik/marketing varyasyonlar olmayacaktır.
- Ekranda ürün adı yalnız **LP FAL Premium** olarak gösterilir.
- Açıklama: **Tek seferlik satın alım · Abonelik değildir.**
- Satın alma CTA'sı: **Premium'a Geç** veya **Premium'u Aç**.
- Kullanıcıya restore/geri yükleme butonu, menüsü veya teknik restore metni gösterilmez.

## 2. Google Play restore — otomatik ve görünmez

- Premium entitlement'ın çevrimiçi kaynağı Google Play satın alma kaydıdır.
- Restore tamamen arka planda, sessiz ve otomatik çalışır.
- Uygulama açılışında, resume'da ve internet tekrar geldiğinde owned purchase durumu sessizce senkronize edilir.
- Aynı Google Play hesabıyla clean install/yeni cihaz sonrasında kullanıcı ayrıca ödeme yapmadan satın alımı otomatik geri alabilmelidir.
- Kullanıcıdan `Satın Alımı Geri Yükle` benzeri bir aksiyon istenmez.
- UI'da restore butonu, restore menüsü veya restore teknik durumu gösterilmez.
- Purchase ve restore aynı entitlement doğrulama hattından geçer.
- Yerel cache tek başına ücretli Premium üretmez.
- Duplicate callback/idempotency korunur.
- Process death sonrasında sonraki açılışta owned purchase sync ile hak tekrar bulunur.
- Otomatik restore başarısızsa uygulama crash/freeze etmez; sonraki uygun lifecycle/network olayında sessizce tekrar dener.

## 3. Premium reklam davranışı

Premium aktifken:
- Rewarded yok.
- Timed interstitial yok.
- App Open yok.
- Banner/native yok.
- Yeni reklam request'i yapılmaz.
- Önceden yüklenmiş reklam nesneleri dispose edilir.
- Boş reklam container'ı bırakılmaz.
- Kahve, Tarot, Rüya Tabiri, El Falı ve bağlı sohbet reklam kapısına takılmaz.

Premium yoksa `MONETIZATION_V1.md` içindeki Free reklam kuralları uygulanır.

## 4. İsim standardı

LP FAL kullanıcı arayüzünde Premium özelliği için tek marka kelimesi **Premium**'dur.

Yasak/legacy kullanıcı metinleri:
- PRO
- Pro
- LP FAL PRO
- Pro'ya Geç
- Pro satın al
- Ömür Boyu Premium
- Lifetime Premium
- Satın Alımı Geri Yükle
- Restore
- Geri Yükle

Doğru örnekler:
- Premium
- LP FAL Premium
- Premium'a Geç
- Premium Aktif
- Tek seferlik satın alım · Abonelik değildir.

`Lefferion Prime` marka adındaki `Prime` bu kurala tabi değildir; bu bir marka adıdır, Premium ürün etiketi değildir.

## 5. Görsel referans kuralı

- `design_refs/ui/` altındaki eski JPG mockup'larda legacy Premium/restore metni görülmesi durumunda bu metin **geçersiz legacy copy** kabul edilir.
- Uygulama runtime'ında mockup screenshot'ı kullanılmayacaktır; gerçek Flutter widget metni daima güncel Premium standardını kullanacaktır.
- Yeni/yenilenen görsel referanslarda `PRO/Pro`, `Ömür Boyu`, `Lifetime`, `Restore` veya `Satın Alımı Geri Yükle` kullanılamaz.
- `assets/ui/` içindeki dekoratif görsellerde ürün plan adı/metni gömülmemesi tercih edilir; plan adı gerekiyorsa yalnız `Premium` kullanılır.

## 6. Release gate

Aşağıdakilerden biri varsa release bloklanır:
- subscription/aylık/yıllık Premium ürünü,
- Premium için `PRO/Pro` kullanıcı metni,
- satın alma sonrası entitlement'ın yalnız local boolean ile verilmesi,
- reinstall/new-device otomatik restore'un çalışmaması,
- kullanıcıya restore/geri yükleme butonu veya teknik restore UI'ı gösterilmesi,
- Premium aktifken herhangi bir reklam request/container/gösterimi,
- satın alma ekranında `Ömür Boyu` veya `Lifetime` paket adı.

**Bu dosya ürün modeli, restore UX'i ve Premium isimlendirmesinde tek kaynak kabul edilir.**