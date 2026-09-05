# LP FAL — V1 Yapılacaklar Listesi

Bu liste `SPECIFICATION.md` şartnamesini uygulamak için sıra bazlı çalışma planıdır. V1 tamamlanana kadar kapsam dışı özellik eklenmez.

---

## FAZ 0 — Repo ve proje temeli

- [ ] Flutter stable proje iskeletini oluştur.
- [ ] Android package/applicationId belirle ve sabitle.
- [ ] `main`, geliştirme branch stratejisi ve `.gitignore` düzenini kur.
- [ ] `analysis_options.yaml` ve lint kurallarını ekle.
- [ ] Feature-first klasör yapısını kur.
- [ ] Environment/secrets yapısını oluştur; hiçbir secret’ı repoya yazma.
- [ ] GitHub Actions: `flutter analyze`, unit/widget test ve Android build kontrolü.
- [ ] Debug APK’nın sıfırdan temiz ortamda derlendiğini doğrula.

**Bitiş kriteri:** Repo temiz, CI yeşil, boş uygulama debug APK olarak açılıyor.

---

## FAZ 1 — Marka, logo ve temel tema

- [ ] MIZANGLOBAL `assets/brand/lefferion-prime-logo.png` asset’ini LP FAL projesine aktar.
- [ ] Uygulama adını her Android/Flutter yüzeyinde `LP FAL` yap.
- [ ] Adaptive launcher icon varyasyonlarını aynı marka geometrisini bozmadan üret.
- [ ] Splash ekranını aynı logoyla oluştur.
- [ ] Açık tema palette/token sistemini tanımla.
- [ ] Tipografi, spacing, radius, shadow ve icon standartlarını tanımla.
- [ ] Dark theme ekleme; V1 yalnız onaylanan açık görünümle ilerlesin.

**Bitiş kriteri:** Launcher, splash ve uygulama içi logolar net; uygulama ismi her yerde LP FAL.

---

## FAZ 2 — Lisanslı gerçek görsel asset seti

- [ ] Dashboard kahve kartı için yüksek kaliteli gerçek fotoğraf seç.
- [ ] Dashboard tarot kartı için premium görsel seç.
- [ ] 78 tarot kartının tek tip yüksek çözünürlüklü/public-domain setini hazırla.
- [ ] Görselleri uygulama için WebP/JPEG/PNG olarak optimize et.
- [ ] Filigranlı, düşük çözünürlüklü, karışık desteli veya AI üretimi görsel kullanma.
- [ ] `assets/licenses/ASSET_LICENSES.md` içine kaynak/lisans kayıtlarını yaz.
- [ ] Görsellerin offline asset olarak açıldığını doğrula.

**Bitiş kriteri:** Uygulamanın tüm statik görselleri lisans açısından kayıtlı, kaliteli ve yerel.

---

## FAZ 3 — Dashboard UI

- [ ] Üst bar: logo + LP FAL + profil/ayar erişimi.
- [ ] Büyük `Kahve Falı` kartını gerçek görselle oluştur.
- [ ] `Falına Bak` CTA’sını kart üzerine doğru kontrastla yerleştir.
- [ ] Büyük `Tarot Falı` kartını yüksek kaliteli görselle oluştur.
- [ ] `Tarot Açılımı` CTA’sını ekle.
- [ ] `Son Falın / Falına Devam Et` kartını conditional yap.
- [ ] Bottom navigation: Ana Sayfa / Fallarım / Premium / Profil.
- [ ] 360dp, orta telefon, büyük telefon, tablet ve BlueStacks responsive kontrolü.
- [ ] Overflow, clipped text ve safe-area sorunlarını temizle.

**Bitiş kriteri:** Dashboard onaylanan açık/krem tasarıma sadık ve tüm hedef ekranlarda responsive.

---

## FAZ 4 — Yerel model altyapısı

- [ ] Resmi Qwen3.5-0.8B model kaynağını ve lisansını sabitle.
- [ ] Android üzerinde kullanılacak inference runtime’ını seç ve küçük PoC hazırla.
- [ ] Q4 ve gerekirse alternatif quantization benchmarkı yap.
- [ ] 4 GB / 6 GB / 8 GB cihaz sınıflarında model yükleme testleri yap.
- [ ] Peak RAM, warm-up, token/s ve crash/OOM sonuçlarını kaydet.
- [ ] Model runtime’ını Flutter UI’dan native/FFI katmanıyla izole et.
- [ ] Inference hiçbir koşulda ana UI thread’ini bloke etmesin.
- [ ] Model missing/corrupt/unsupported-device state’lerini tasarla.

**Bitiş kriteri:** Telefonda internetsiz basit Türkçe prompta Qwen cevap verebiliyor ve UI donmuyor.

---

## FAZ 5 — Debug model kurulum yolu

- [ ] Modeli Git reposuna commit etme.
- [ ] Debug uygulamada local model path desteği ekle.
- [ ] Windows için `tools/install_model.ps1` veya eşdeğer ADB kurulum aracı hazırla.
- [ ] Model dosyasını application private storage’a kopyalama/tespit akışını kur.
- [ ] SHA-256 bütünlük kontrolü ekle.
- [ ] Google Play’e yüklemeden temiz telefonda model + APK testini doğrula.

**Bitiş kriteri:** Geliştirici PC’den modeli bir kez telefona aktararak tüm AI fonksiyonlarını Play Store olmadan test edebiliyor.

---

## FAZ 6 — Kahve fotoğrafı giriş ve kalite kontrolü

- [ ] Kamera izni ve galeri seçimi.
- [ ] 2–3 fotoğraf desteği; 3 fotoğraf önerisi.
- [ ] EXIF/orientation düzeltme.
- [ ] Görsel çözünürlük/boyut optimizasyonu.
- [ ] Blur kontrolü.
- [ ] Exposure/karanlık kontrolü.
- [ ] Fincan içi görünürlük kontrolü.
- [ ] Yanlış görsel tespiti.
- [ ] Kullanıcıya fotoğraf çekim yönergeleri.
- [ ] Kalitesiz görüntüde AI’ya geçmeden retry.

**Bitiş kriteri:** AI’ya yalnız analiz edilebilir fincan fotoğrafları gidiyor.

---

## FAZ 7 — Kahve Falı AI pipeline

- [ ] Görüntü preprocessing katmanı.
- [ ] Fincan bölgesi crop/normalize.
- [ ] Ağız / orta / dip / kulp çevresi gibi gerekli alt bölgeleri çıkar.
- [ ] İlk Qwen aşaması: sadece structured visual analysis.
- [ ] `region`, `shape`, `confidence` veri modelini tanımla.
- [ ] Confidence threshold kurallarını sabitle.
- [ ] Düşük güvenli sembolleri ele.
- [ ] İkinci Qwen aşaması: yalnız structured analysis üzerinden fal metni üret.
- [ ] Genel / Aşk / İş-Para / Yol-Değişim bölümlerini oluştur.
- [ ] Fal dışı veya yüksek riskli kesin iddiaları filtrele.
- [ ] Aynı görselde gereksiz/random sembol eklenmesini engelle.
- [ ] Analiz ekranında gerçek ilerleme state’leri göster; sahte % bar kullanma.

**Bitiş kriteri:** Kullanıcı fotoğraf yükleyip cihaz üzerinde tutarlı kahve falı sonucu alabiliyor.

---

## FAZ 8 — Qwen doğruluk doğrulaması

- [ ] En az 100 gerçek fincan görselinden sabit test seti oluştur.
- [ ] İnsan etiketli belirgin sembol/bölge ground-truth kaydı ekle.
- [ ] High-confidence precision metriğini ölç.
- [ ] False-positive metriğini ölç.
- [ ] Aynı fotoğraf tekrar-test determinism/regresyon kontrolü.
- [ ] En az 50 Türkçe fal/chat sabit prompt seti hazırla.
- [ ] Tekrar, anlamsızlık, dil bozulması ve fal dışına çıkma testleri.
- [ ] Gerekirse prompt/pipeline/fine-tune iterasyonu yap.
- [ ] Hedef: high-confidence precision ≥ %80.
- [ ] Hedef: high-confidence false-positive ≤ %15.

**Bitiş kriteri:** Şartnamedeki kalite hedefleri karşılanmadan kahve AI modülü final işaretlenmez.

---

## FAZ 9 — Tarot

- [ ] 78 kart metadata’sını tanımla.
- [ ] Kart adlarının Türkçe standardını sabitle.
- [ ] Düz/ters kart state’i ekle.
- [ ] Tek Kart açılımı.
- [ ] Üç Kart: Geçmiş / Şimdi / Gelecek.
- [ ] Beş Kart detaylı açılım.
- [ ] Kart seçme/çekme animasyonlarını hafif ve akıcı yap.
- [ ] Seçilen kart ID/ad/pozisyon/düz-ters bilgisini Qwen’e structured olarak ver.
- [ ] Qwen’in kartı görselden yeniden tanımaya çalışmasını engelle.
- [ ] Tarot yorum ekranını oluştur.

**Bitiş kriteri:** 1/3/5 kart açılımları hatasız çalışıyor, kart görselleri premium kalite ve yorumlar doğru kart state’i üzerinden geliyor.

---

## FAZ 10 — Fal sohbeti

- [ ] Her fal için benzersiz local conversation oluştur.
- [ ] Kahve structured analysis + final yorum context’e bağla.
- [ ] Tarot kart metadata + final yorum context’e bağla.
- [ ] Fal dışı isteklere scope rejection ekle.
- [ ] Prompt injection testleri ekle.
- [ ] Sohbet geçmişini cihazda sakla.
- [ ] Context büyüdüğünde local özetleme stratejisi uygula.
- [ ] Chat UI: yazıyor state’i, gönderme, hata/retry.
- [ ] Offline chat testini yap.

**Bitiş kriteri:** AI yalnız aktif fal hakkında doğal Türkçe sohbet ediyor ve genel asistana dönüşmüyor.

---

## FAZ 11 — Geçmiş Fallar ve yerel storage

- [ ] Local database/storage seç.
- [ ] Kahve falını kaydet.
- [ ] Tarot falını kaydet.
- [ ] Chat geçmişini fala bağla.
- [ ] Son falı dashboard’da göster.
- [ ] Tek fal silme.
- [ ] Tüm geçmişi silme.
- [ ] Uygulama verisini sıfırlama.
- [ ] Gereksiz fotoğrafları varsayılan olarak kalıcı tutmama politikasını uygula.

**Bitiş kriteri:** Kullanıcı verileri server olmadan cihazda yönetiliyor ve tamamen silinebiliyor.

---

## FAZ 12 — Reklam monetizasyonu

- [ ] Google AdMob entegrasyonu.
- [ ] Consent/UMP akışı gereken bölgeler için ekle.
- [ ] Kahve sonucu açma Rewarded Ad akışı.
- [ ] Tarot sonucu açma Rewarded Ad akışı.
- [ ] Fal sohbetinde mesaj paketi için Rewarded Ad akışı.
- [ ] Dashboard/geçmiş için ölçülü banner/native alanları.
- [ ] Interstitial yalnız doğal geçiş noktalarında.
- [ ] Frequency-cap tanımla ve test et.
- [ ] Reklam load timeout/retry/fallback mekanizması.
- [ ] Reklam yüklenmezse kullanıcıyı dead-end’de bırakma.
- [ ] Premium entitlement varsa hiçbir reklam çağrısı/görünümü oluşturma.

**Bitiş kriteri:** Ücretsiz akış gelir üretiyor ama kullanım engellenmiyor; reklam spam’i ve accidental click riski yok.

---

## FAZ 13 — Premium aylık reklamsız abonelik

- [ ] Google Play Billing güncel entegrasyonu.
- [ ] Tek ürün: aylık Premium / reklamsız.
- [ ] Premium ekranı onaylanan UI yönünde.
- [ ] Abonelik satın alma.
- [ ] Satın alımları geri yükleme.
- [ ] Entitlement cache/state yönetimi.
- [ ] Abonelik sona erdiğinde ücretsiz reklamlı moda güvenli dönüş.
- [ ] Premium aktifken tüm banner/rewarded/interstitial noktalarını kapat.
- [ ] Sandbox/test purchase senaryoları.

**Bitiş kriteri:** Premium tek iş yapıyor: reklamları kaldırıyor; satın alma ve restore sağlam.

---

## FAZ 14 — Güvenlik, gizlilik ve Google Play uygunluğu

- [ ] Eğlence amaçlı fal açıklamasını uygun onboarding/ayar alanına koy.
- [ ] Sağlık/ölüm/hamilelik/hukuk/garantili finansal sonuç gibi kesin iddiaları filtrele.
- [ ] AI output report/bildirme mekanizması.
- [ ] Privacy Policy hazırla.
- [ ] Google Play Data Safety beyanını gerçek SDK davranışlarına göre doldur.
- [ ] Açık kaynak lisans ekranı.
- [ ] Qwen Apache-2.0 attribution/NOTICE gereksinimlerini ekle.
- [ ] Üçüncü taraf görsel lisans listesini ekle.
- [ ] Secrets taraması.
- [ ] Loglarda kullanıcı fotoğrafı/chat/prompt sızıntısı olmadığını doğrula.

**Bitiş kriteri:** Mağaza politikaları, lisans ve gizlilik tarafında bilinen bloklayıcı eksik kalmıyor.

---

## FAZ 15 — Release model dağıtımı

- [ ] Google Play on-demand model/asset pack yapısını ekle.
- [ ] Modeli ana APK içine gömme.
- [ ] İlk kullanımda model availability state’i.
- [ ] İndirme ilerleme/hata/retry UI.
- [ ] Model bir kez indiğinde offline inference.
- [ ] SHA-256 / version doğrulaması.
- [ ] `bundletool` local-testing ile Play’e yüklemeden asset delivery testini yap.
- [ ] Internal test track ile gerçek Play dağıtım testini final aşamada yap.

**Bitiş kriteri:** Debug local model ve release Play asset modeli aynı AI interface üzerinden çalışıyor.

---

## FAZ 16 — Performans ve cihaz QA

- [ ] 4 GB gerçek Android cihaz testi.
- [ ] 6 GB gerçek Android cihaz testi.
- [ ] 8 GB+ gerçek Android cihaz testi.
- [ ] Düşük/orta/yüksek Android SoC testleri mümkün olduğunca yap.
- [ ] Tablet testi.
- [ ] BlueStacks testi.
- [ ] App cold start ölç.
- [ ] Qwen warm-up ölç.
- [ ] İlk token süresi ölç.
- [ ] Kahve tam analiz süresi ölç.
- [ ] Peak RAM ölç.
- [ ] 20+ ardışık fal stress testi.
- [ ] Uzun chat memory leak testi.
- [ ] Background → foreground lifecycle testi.
- [ ] Low-memory kill/recovery testi.
- [ ] Uçak modu AI testi.

**Bitiş kriteri:** Crash/OOM/bloklayan performans problemi yok; UI AI sırasında responsive.

---

## FAZ 17 — Ağ izolasyon testi

- [ ] Model kurulduktan sonra kahve AI trafiğini network inspector ile izle.
- [ ] Tarot AI trafiğini izle.
- [ ] Fal chatbot trafiğini izle.
- [ ] Fotoğraf/prompt/chat için outbound request olmadığını doğrula.
- [ ] Yalnız AdMob ve Google Play/Billing gibi izin verilen servislerin ağ erişimini doğrula.
- [ ] Debug log/analytics üzerinden hassas içerik sızıntısı olmadığını kontrol et.

**Bitiş kriteri:** AI katmanı %100 on-device doğrulandı.

---

## FAZ 18 — Son regresyon ve release

- [ ] `SPECIFICATION.md` final checklist’i madde madde kontrol et.
- [ ] Tüm unit/widget/integration testlerini çalıştır.
- [ ] `flutter analyze` sıfır bloklayıcı hata.
- [ ] Debug APK temiz build.
- [ ] Signed release APK temiz build.
- [ ] Signed release AAB temiz build.
- [ ] Temiz cihazda APK install/open testi.
- [ ] Upgrade testi.
- [ ] Uninstall/reinstall testi.
- [ ] Model missing/corrupt/reinstall senaryoları.
- [ ] AdMob test ID → production ID geçiş kontrolü.
- [ ] Billing test → production product ID kontrolü.
- [ ] VersionCode/VersionName kontrolü.
- [ ] Store listing metinleri/görselleri.
- [ ] Privacy Policy/Data Safety son kontrol.
- [ ] Final commit/tag oluştur.
- [ ] Teslim APK + AAB + test raporu + commit SHA’yı kaydet.

**Bitiş kriteri:** Şartname checklist’inde açık madde yok, temiz build kuruluyor ve uçtan uca gerçek cihaz testi geçiyor.

---

# BLOKLAYICI “OLMAZSA OLMAZ” LİSTESİ

Aşağıdakilerden biri eksikse uygulama final değildir:

1. **LP FAL adı ve Mizan/Lefferion Prime logosu doğru kullanılmalı.**
2. **Onaylanan açık/krem dashboard tasarım yönü korunmalı.**
3. **AI üretimi uygulama içi statik görsel kullanılmamalı.**
4. **Kahve fotoğrafı gerçek biçimde analiz edilmeli; tek promptla uydurma fal üretilmemeli.**
5. **Structured visual analysis → fal üretimi iki aşamalı olmalı.**
6. **Qwen lokal cihazda çalışmalı; Cloudflare/harici inference olmamalı.**
7. **Testte model Google Play’e ihtiyaç duymadan local kurulabilmeli.**
8. **Release’te model ana APK içine gömülmemeli; Play on-demand dağıtımı kullanılmalı.**
9. **4/6/8 GB RAM sınıflarında QA yapılmalı.**
10. **Tarot 1/3/5 kart akışı çalışmalı ve kart seti yüksek kaliteli/lisansı temiz olmalı.**
11. **Fal chatbot yalnız mevcut fal bağlamında konuşmalı.**
12. **Rewarded reklamlar kahve, tarot ve chat ekonomisine sağlam bağlanmalı.**
13. **Premium yalnız aylık reklamsız abonelik olarak sade tutulmalı.**
14. **Premium restore ve entitlement çalışmalı.**
15. **Reklam yükleme hatası uygulamayı kilitlememeli.**
16. **AI fotoğraf/prompt/chat verisini sunucuya göndermemeli.**
17. **Geçmiş fallar yerelde saklanıp kullanıcı tarafından silinebilmeli.**
18. **Qwen doğruluk test seti ve Türkçe regresyon testleri release gate olmalı.**
19. **Yüksek riskli kesin fal iddiaları filtrelenmeli.**
20. **Gizlilik, lisans, Google Play AI ve reklam/billing gereksinimleri tamamlanmalı.**
21. **Tablet/BlueStacks dahil responsive taşma olmamalı.**
22. **Signed APK ve AAB temiz build/install testinden geçmeli.**
23. **V1 bitene kadar kapsam genişletilmemeli.**

---

# ÇALIŞMA KURALI

Her faz tamamlandığında yalnız checkbox işaretlemek yeterli değildir. İlgili fazın **bitiş kriteri test edilerek doğrulanmalıdır**. Test kanıtı olmayan madde tamamlanmış sayılmaz.

Yeni özellik fikri çıkarsa `V2_BACKLOG.md` içine not edilebilir; **V1 koduna eklenmez**.
