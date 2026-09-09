# LP FAL — Rüya Tabiri V1 Şartnamesi

**Durum:** ZORUNLU / normatif  
**Karar tarihi:** 2026-09-09  
**Kapsam:** Rüya Tabiri ürün akışı, yerel AI pipeline, güvenlik, gizlilik, sohbet, monetizasyon ve QA

> Bu dosya Rüya Tabiri konusunda `SPECIFICATION.md`, `TODO.md`, `V1_RELEASE_GATES.md`, `MONETIZATION_V1.md` ve eski mockup metinlerindeki çelişkili kapsam ifadelerini geçersiz kılar. Rüya Tabiri artık V1 ana modülüdür.

---

## 1. V1 ana fal/yorum modülleri

LP FAL V1 ana içerik modülleri artık:
1. **Kahve Falı**
2. **Tarot Falı**
3. **Rüya Tabiri**
4. **El Falı**

Rüya Tabiri genel amaçlı AI asistanı değildir. Kullanıcının anlattığı rüyanın geleneksel/sembolik öğelerini eğlence ve kişisel yorum amacıyla açıklayan kontrollü bir modüldür.

---

## 2. Ürün konumu ve kullanıcıya verilen vaat

Rüya Tabiri:
- geleceği bilen bir sistem olarak sunulmaz,
- bilimsel psikolojik analiz olarak sunulmaz,
- tıbbi/psikiyatrik değerlendirme değildir,
- dini hüküm/fetva/ilahi mesaj doğrulama sistemi değildir,
- hukuki veya finansal danışmanlık değildir,
- rüyanın gerçek hayatta kesin bir olayın habercisi olduğunu iddia etmez.

Kullanıcıya sunulan değer: rüyada anlattığı öğeler üzerinden **sembolik, kültürel ve eğlence amaçlı bir yorum**.

Zorunlu kullanıcı dili örnekleri:
- `Sembolik olarak...`
- `Geleneksel rüya yorumlarında...`
- `Bu öğe ... temasını çağrıştırabilir.`
- `Bu, kesin bir anlam veya gelecek tahmini değildir.`

---

## 3. Kullanıcı girdisi

### 3.1 Ana giriş
- Çok satırlı gerçek metin alanı.
- Kullanıcı rüyasını doğal Türkçeyle anlatır.
- Metin boşsa yorum başlatılmaz.
- Aşırı uzun girdiler merkezi token/karakter limitiyle sınırlandırılır; UI kalan/uygun uzunluğu anlaşılır biçimde yönetir.
- Kullanıcı gönderene kadar metin AI'a verilmez.

### 3.2 Opsiyonel bağlam
Kullanıcı isterse ekleyebilir:
- rüyada baskın his: korku / huzur / şaşkınlık / üzüntü / mutluluk / belirsiz,
- tekrar eden rüya olup olmadığı,
- rüyanın kendisi için önemli gördüğü kısa bağlam.

Bu alanlar zorunlu değildir. Uygulama kullanıcıdan sağlık, din, cinsel yaşam, siyasi görüş, biyometrik veri veya başka hassas kişisel veri istemek üzere tasarlanmaz.

### 3.3 Girdi gizliliği
- Rüya metni varsayılan olarak cihazda işlenir.
- AI inference için rüya metni harici sunucuya gönderilmez.
- Production loglarına ham rüya metni yazılmaz.
- Clipboard içeriği kullanıcı açıkça yapıştırmadan okunmaz.
- Analytics event'lerine rüya metni veya türetilmiş hassas içerik yazılmaz.

---

## 4. AI pipeline — iki aşamalı ve grounded

### Aşama A — Structured Dream Extraction
Qwen ilk aşamada yorum yazmaz. Yalnız kullanıcının gerçekten anlattığı içerikten structured veri çıkarır.

Örnek şema:
```json
{
  "scenes": ["old_house", "dark_corridor"],
  "symbols": ["door", "water"],
  "emotions": ["uncertainty", "fear"],
  "people_or_roles": ["unknown_person"],
  "recurring": false,
  "user_context": null,
  "uncertain_items": []
}
```

Kurallar:
- Kullanıcının söylemediği sahne/sembol eklenmez.
- Kişi kimliği tahmin edilmez.
- Gerçek hayattaki kişi hakkında suç, sadakat, hastalık veya niyet çıkarımı yapılmaz.
- Belirsiz ifade belirsiz olarak işaretlenir.
- Rüya anlatımı çok kısa/anlamsızsa model bunu dürüstçe belirtir; ayrıntı uydurmaz.

### Aşama B — Symbolic Interpretation
İkinci Qwen aşaması yalnız structured dream verisini ve kullanıcı tarafından verilmiş opsiyonel bağlamı kullanır.

Çıktı bölümleri:
1. **Rüyanın Kısa Özeti** — kullanıcının anlattıklarını tarafsızca özetler.
2. **Öne Çıkan Semboller** — yalnız gerçek girdide bulunan öğeler.
3. **Duygusal Atmosfer** — yalnız anlatılan veya kullanıcı tarafından seçilen duygular üzerinden.
4. **Sembolik Temalar** — koşullu, kültürel/sembolik dil.
5. **Genel Sembolik Yorum** — kesinlik içermeyen birleşik yorum.
6. **Belirsizlik Notu** — rüya yorumlarının kesin/bilimsel sonuç olmadığını açıklar.

Rüya sonucunda `Tavsiye`, `Öneri`, `Ne Yapmalısın`, `Karar` veya profesyonel danışmanlık bölümü bulunmaz.

---

## 5. Rüya Tabiri için kesin yasaklar

Model ve uygulama-level filtre aşağıdakileri engeller:

### 5.1 Gelecek / kehanet
- `Kesin olacak.`
- `Şu tarihte olacak.`
- evlilik, ayrılık, ölüm, kaza, işten çıkarılma, para kazanma/kaybetme gibi geleceğe ilişkin kesin hüküm.

### 5.2 Sağlık / psikoloji
- hastalık teşhisi,
- psikiyatrik/psikolojik tanı,
- hamilelik/doğurganlık çıkarımı,
- ilaç/tedavi tavsiyesi,
- `Bu rüya şu hastalığın belirtisidir` türü iddia.

Rüyadaki sağlık teması, gerçek sağlık durumunun kanıtı gibi yorumlanmaz.

### 5.3 Hukuk / finans
- dava sonucu tahmini,
- suçluluk/masumiyet hükmü,
- yatırım, kredi, bahis veya finansal karar yönlendirmesi,
- `bu rüya para yatırman gerektiğini gösteriyor` gibi öneriler.

### 5.4 İlişki ve önemli yaşam kararları
- `ayrıl`, `evlen`, `işini bırak`, `taşın`, `iletişimi kes`, `borç al` gibi direktifler yok.
- Başka bir kişinin sadakati, sevgisi, ihaneti veya niyeti rüyadan kesinleştirilmez.

### 5.5 Din / doğaüstü kesinlik
- rüya `ilahi emir`, `kesin mesaj`, `vahiy`, `cin`, `büyü`, `lanet`, `nazarın kesin kanıtı` olarak doğrulanmaz.
- Dini otorite/fetva rolü üstlenilmez.
- Kullanıcının inancı küçümsenmez; fakat metafizik iddia gerçek olgu olarak onaylanmaz.

### 5.6 Hassas özellik çıkarımı
Rüya metninden kullanıcı veya üçüncü kişiler hakkında:
- ırk/etnik köken,
- din,
- siyasi görüş,
- cinsel yönelim/cinsel yaşam,
- sağlık durumu,
- suç geçmişi
çıkarımı yapılmaz; kullanıcı açıkça yazmış olsa bile bu bilgi fal/yorum amacı dışında profilleme için kullanılmaz.

---

## 6. Yüksek riskli rüya/chat davranışı

### Kendine zarar verme / intihar
Kullanıcı rüyasını anlatırken gerçek hayatta kendine zarar verme niyeti veya yakın risk ifade ederse sistem:
- rüyayı kehanet gibi yorumlamaz,
- güvenlik odaklı destekleyici yanıt moduna geçer,
- tehlikeyi büyüten mistik açıklama yapmaz,
- gerekli acil destek yönlendirmesini ürün safety katmanına bırakır.

### Şiddet / suç
Rüyadaki şiddet otomatik olarak gerçek niyet sayılmaz. Kullanıcı gerçek dünyada zarar verme/suç işleme niyeti açıklarsa fal yorumu yerine güvenli içerik politikasına göre yanıt üretilir.

### Paranoya / doğaüstü tehdit
`Beni takip ediyorlar`, `bana büyü yapıldı`, `rüyam bunun kanıtı` gibi gerçek dünyaya taşınan kesin paranoyak/doğaüstü iddialar doğrulanmaz. Rüya ile gerçek olay arasında kanıt ilişkisi kurulmaz.

---

## 7. Rüya Sohbeti

Rüya sonucu sonrası kullanıcı aynı rüya için AI ile konuşabilir.

Context:
- original dream text veya güvenli yerel özeti,
- structured dream extraction,
- final symbolic interpretation,
- sınırlandırılmış conversation history.

Kurallar:
- Yalnız aktif rüya bağlamında konuşur.
- Genel amaçlı ChatGPT değildir.
- `Buna göre ne yapmalıyım?` sorusunda karar/tavsiye üretmez; sembolik yorumu açıklayabilir.
- Kullanıcı yeni bir rüya anlatırsa yeni dream session/context açılması önerilen ürün davranışıdır.
- Chat çıktısı da global compliance/safety filtresinden geçer.

Free chat monetizasyonu diğer fal sohbetiyle aynıdır:
- ilk takip sorusu ücretsiz,
- sonrasında her 3 kullanıcı mesajı paketi için 2 Rewarded Ad.
- Premium kullanıcıda reklam kapısı yok.

---

## 8. Geçmiş / yerel kayıt

Geçmiş kaydında tutulabilecekler:
- `type = dream`,
- oluşturulma tarihi,
- kullanıcı tarafından verilen başlık varsa başlık,
- rüya metni veya kullanıcının tercihine göre güvenli yerel özeti,
- structured extraction,
- final symbolic interpretation,
- rüyaya bağlı chat geçmişi.

Zorunlu:
- kullanıcı kaydı silebilir,
- silme rüya + bağlı chat için uygulanır,
- ham rüya metni analytics/log sistemine gitmez,
- hassas veriler reklam hedefleme segmentine dönüştürülmez,
- Auto Backup davranışı privacy tasarımına göre açıkça kontrol edilir.

---

## 9. UI akışı

Dashboard kartı:
- **Rüya Tabiri**
- CTA örnekleri: `Rüyanı Anlat` / `Rüyanı Yorumla`

Rüya giriş ekranı:
- başlık: `Rüya Tabiri`,
- geniş metin alanı,
- opsiyonel duygu seçimi,
- `Yorumla` CTA,
- görünür kısa entertainment bildirimi,
- gerekirse Premium/Free reklam kapısı yorum üretilmeden güvenli akışta.

Analiz state:
- sahte yüzde yok,
- gerçek state örnekleri: `Rüyan okunuyor` → `Semboller ayrıştırılıyor` → `Sembolik yorum hazırlanıyor`.

Sonuç ekranı:
- Bölüm 4. Aşama B çıktıları,
- `Rüya hakkında sohbet et` CTA,
- `AI içeriğini bildir` aksiyonu,
- sabit disclaimer.

---

## 10. Monetizasyon

Free kullanıcı:
- Rüya Tabiri tam sonucu = **2 Rewarded Ad**.
- İlk reklam tek başına sonucu açmaz.
- `0/2 → 1/2 → 2/2` transaction mantığı diğer modüllerle aynıdır.
- Reward transaction başka rüyaya/fala taşınamaz.

Premium:
- Rüya Tabiri doğrudan kullanılır.
- Rewarded, timed interstitial, App Open, banner/native yok.

Timed interstitial:
- rüya yazarken,
- metin gönderilirken,
- Qwen analiz/yorum üretirken,
- sonucu aktif okurken,
- dream chat yazarken/AI üretirken
asla gösterilmez.

Banner:
- rüya giriş/sonuç/chat yüzeyinde gösterilmez.

---

## 11. Gizlilik ve veri minimizasyonu

Rüya metni kişisel veya hassas bilgiler içerebilir. Bu nedenle:
- yalnız ürün işlevi için gerekli veri işlenir,
- kullanıcıdan gereksiz profil bilgisi istenmez,
- on-device inference varsayılandır,
- harici AI API yok,
- production log redaction zorunlu,
- kullanıcıya veri işleme amacı ve saklama davranışı privacy/aydınlatma metninde açıklanır,
- kullanıcı tarafından başlatılan AI report dışında içerik sunucuya gönderilmez,
- report payload varsayılan olarak minimumdur; kullanıcı metninin tamamı otomatik eklenmez.

---

## 12. AI report / flag

Her Rüya Tabiri sonucu ve dream chat AI mesajında erişilebilir bir `Bildir`/flag aksiyonu bulunur.

- Kullanıcı uygulamadan çıkmadan AI çıktısını raporlayabilir.
- Report yalnız kullanıcı aksiyonuyla oluşur.
- Varsayılan payload: içerik türü, safety kategori seçimi, model/prompt sürümü, teknik hata kodu ve kullanıcı tarafından paylaşılması seçilen minimum içerik.
- Ham rüya metni veya kişisel bilgi varsayılan olarak report'a eklenmez.
- Raporlar safety regresyonu ve filtre geliştirmede kullanılabilir; kullanıcı verisi bunun ötesinde training verisine otomatik dönüşmez.

---

## 13. Prompt sürümleme

Yeni prompt aileleri:
- `dream_extract`
- `dream_interpret`
- `dream_chat`

Global:
- `safety`
- `compliance`

Her release kaydı:
- Qwen model/revision,
- runtime,
- quantization,
- dream prompt sürümleri,
- safety/compliance sürümü,
- generation params.

Prompt/model değişince Dream QA regresyonu tekrar koşar.

---

## 14. Rüya QA seti — release bloklayıcı

En az 100 Türkçe rüya senaryosu içeren sabit test seti hazırlanır. Set yalnız normal örneklerden oluşmaz.

Zorunlu kategoriler:
- kısa/belirsiz rüya,
- çok uzun rüya,
- birden fazla sahne,
- korku/kabus,
- ölüm teması,
- hastalık/hamilelik teması,
- ilişki/ihanet sorusu,
- para/yatırım/bahis sorusu,
- dava/suç sorusu,
- dini/doğaüstü kesinlik talebi,
- büyü/nazar/paranoya talebi,
- gerçek hayatta kendine zarar verme sinyali,
- gerçek hayatta şiddet/suç niyeti,
- prompt injection/jailbreak,
- kullanıcının `kesin söyle` baskısı,
- `ne yapmalıyım` yönlendirme talebi,
- rüyada olmayan ayrıntıyı modele ekletmeye çalışma.

Release hedefi:
- uydurulmuş rüya ayrıntısı yok,
- kesin gelecek iddiası yok,
- tıbbi/psikolojik teşhis yok,
- profesyonel/hayati tavsiye yok,
- doğaüstü iddiayı gerçek olarak doğrulama yok,
- kullanıcı tarafından bildirilebilir AI çıktı UI'ı çalışıyor.

---

## 15. Zorunlu disclaimer

Rüya sonucu ve uygun chat yüzeylerinde görünür kısa metin:

**“Bu içerik eğlence ve kişisel yorum amaçlıdır; tıbbi, psikolojik, hukuki, finansal, dini veya diğer profesyonel danışmanlık yerine geçmez ve kesin gelecek tahmini değildir.”**

Metin kullanıcıyı korkutmak için büyütülmez fakat saklanmaz veya erişilemez hale getirilmez.

---

# BLOKLAYICI ÖZET

Rüya Tabiri final kabul edilmez, eğer:
1. Rüya V1 dashboard/history/chat akışlarına gerçek modül olarak bağlı değilse.
2. AI kullanıcı söylemediği rüya ayrıntısını uyduruyorsa.
3. Sonuç tavsiye/karar/danışmanlık dili kullanıyorsa.
4. Sağlık/psikoloji/hukuk/finans/din alanında otorite iddiası varsa.
5. Kesin gelecek veya doğaüstü kesinlik üretiliyorsa.
6. Dream text production log/analytics'e sızıyorsa.
7. AI report/flag akışı yoksa.
8. Free/Premium monetizasyonu merkezi kurallarla uyumlu değilse.
9. Dream chat genel amaçlı asistana dönüşüyorsa.
10. Global `COMPLIANCE_BY_DESIGN.md` gate'leri sağlanmıyorsa.
