# LP FAL — V1 Yapılacaklar Listesi

Bu liste `SPECIFICATION.md`, `V1_RELEASE_GATES.md` ve `MONETIZATION_V1.md` ile birlikte uygulanır. Sıra bağımlılıklara göre düzenlenmiştir. Bir fazın bitiş kriteri doğrulanmadan sonraki kritik faz final kabul edilmez.

---

## FAZ 0 — Repo, kimlik ve çalışma kuralları

- [ ] Flutter stable proje iskeletini oluştur.
- [ ] Android `applicationId` değerini sabitle (`com.lefferionprime.lpfal` önerilen kalıcı kimlik).
- [ ] `minSdk/compileSdk/targetSdk` değerlerini AI runtime + güncel Play gereksinimine göre belirle.
- [ ] `.gitignore`, lint ve `analysis_options.yaml` kur.
- [ ] Feature-first klasör yapısını oluştur.
- [ ] Secrets/release signing ayrımını kur; keystore veya secret repoya yazma.
- [ ] GitHub Actions: analyze + test + Android build kontrolü.
- [ ] V1 kapsam dokümanlarını repo kökünde normatif kabul et.

**Bitiş kriteri:** Temiz repo, CI çalışıyor, boş debug APK derlenip açılıyor.

---

## FAZ 1 — Marka + hukuki/güvenli ürün temeli

- [ ] MIZANGLOBAL `assets/brand/lefferion-prime-logo.png` asset'ini LP FAL'a aktar.
- [ ] Görünen adı her yerde `LP FAL` yap.
- [ ] Adaptive launcher icon ve splash'ı aynı marka geometrisiyle hazırla.
- [ ] Uygulamanın eğlence/kişisel yorum konumunu sabitle.
- [ ] Ortak safety policy oluştur:
  - kesin gelecek iddiası yok,
  - tıbbi/hukuki/finansal/profesyonel tavsiye yok,
  - hayat kararı yönlendiren emir yok,
  - ölüm/hamilelik/hastalık/hukuki sonuç/garantili finans kesinliği yok.
- [ ] Sonuç ekranında kullanılacak disclaimer metnini component olarak tanımla.
- [ ] AI prompt katmanından bağımsız uygulama-level safety filtre arayüzünü tasarla.

**Bitiş kriteri:** Marka sabit, safety kuralları dokümante ve kod mimarisinde ayrı bir katman olarak tanımlı.

---

## FAZ 2 — Görsel asset seti + design system

- [ ] Açık/krem renk tokenlarını tanımla.
- [ ] Tipografi, spacing, radius, shadow, button ve card standartlarını oluştur.
- [ ] Kahve dashboard görseli için lisanslı gerçek fotoğraf seç.
- [ ] Tarot dashboard görseli + 78 kartlık tek yüksek kaliteli/public-domain deste hazırla.
- [ ] El Falı dashboard görseli için lisanslı gerçek avuç içi fotoğrafı seç.
- [ ] AI üretimi dekoratif asset kullanma.
- [ ] Görselleri WebP/JPEG/PNG olarak optimize et.
- [ ] `assets/licenses/ASSET_LICENSES.md` kayıtlarını oluştur.

**Bitiş kriteri:** Tasarım tokenları hazır, bütün V1 statik görsel kaynakları lisanslı ve yerel.

---

## FAZ 3 — Navigation + Dashboard UI

- [ ] Üst bar: logo + LP FAL + profil/ayar.
- [ ] Büyük Kahve Falı hero kartı: `Falına Bak`.
- [ ] Tarot Falı kartı: `Kartlarını Seç`.
- [ ] El Falı kartı: `Avucunu Yorumla`.
- [ ] Dar telefonda dikey layout.
- [ ] Geniş telefon/tablette Tarot + El Falı responsive iki kolon/grid varyasyonu.
- [ ] Son Falın / Sohbete Dön conditional kartı.
- [ ] Bottom nav: Ana Sayfa / Fallarım / Premium / Profil.
- [ ] 360dp, orta/büyük telefon, tablet, BlueStacks overflow kontrolü.

**Bitiş kriteri:** Üç ana fal modu dashboard'dan erişilebilir ve onaylı açık tasarım bütün hedef ekranlarda bozulmuyor.

---

## FAZ 4 — Yerel Qwen runtime PoC

- [ ] Resmi Qwen3.5-0.8B model/revision ve lisansı sabitle.
- [ ] Android'de kullanılacak inference runtime'ını seç.
- [ ] Görsel input gerektiren Kahve + El için runtime/model paketinin multimodal input desteğini doğrula.
- [ ] Q4 ve gerekiyorsa alternatif quantization benchmarkı yap.
- [ ] 4/6/8 GB cihaz sınıflarında model load testi.
- [ ] Peak RAM, warm-up, token/s, OOM sonuçlarını kaydet.
- [ ] Flutter UI ↔ native/FFI inference katmanını ayır.
- [ ] Inference ana thread'i bloke etmesin.
- [ ] Model missing/corrupt/unsupported-device state'lerini tanımla.

**Bitiş kriteri:** Telefonda internetsiz Türkçe text prompt ve test görsel input işlenebiliyor; UI donmuyor.

---

## FAZ 5 — Debug/local model kurulumu

- [ ] Modeli Git repo geçmişine commit etme.
- [ ] Debug local model path desteği.
- [ ] Windows `tools/install_model.ps1` veya eşdeğer ADB aracı.
- [ ] App-private model storage.
- [ ] Model/projector SHA-256 doğrulaması.
- [ ] Google Play'e yüklemeden clean phone model + APK testi.

**Bitiş kriteri:** Model PC'den bir kez telefona kurulup bütün AI geliştirmesi Play olmadan test edilebiliyor.

---

## FAZ 6 — Ortak kamera / Photo Picker / gizlilik pipeline'ı

Bu faz Kahve ve El Falı tarafından ortak kullanılacaktır.

- [ ] System Photo Picker entegrasyonu.
- [ ] Kamera capture entegrasyonu.
- [ ] Gereksiz broad storage permission isteme.
- [ ] Kamera iznini yalnız kullanıcı capture başlatınca iste.
- [ ] EXIF/orientation normalize.
- [ ] App-private temp file yönetimi.
- [ ] Blur/exposure/resolution için ortak kalite yardımcıları.
- [ ] Fotoğraf cleanup lifecycle.
- [ ] Production log redaction.
- [ ] Android Auto Backup exclusion.
- [ ] Kullanıcı fotoğraflarının training'e varsayılan olarak gitmediğini garanti et.

**Bitiş kriteri:** Fotoğraf alma, işleme ve silme akışı güvenli; Kahve/El aynı media abstraction'ını kullanıyor.

---

## FAZ 7 — Kahve fotoğrafı kalite + preprocessing

- [ ] 2–3 fotoğraf desteği; 3 önerisi.
- [ ] Fincan/fincan içi görünürlük kontrolü.
- [ ] Yanlış görsel tespiti.
- [ ] Crop/normalize.
- [ ] Ağız/orta/dip/kulp çevresi region extraction.
- [ ] Kalitesiz fotoğrafta AI başlamadan kullanıcıya retry.
- [ ] Fotoğraf çekim yönergesi ekranı.

**Bitiş kriteri:** Structured analysis'e yalnız yeterli kalitedeki fincan görüntüleri giriyor.

---

## FAZ 8 — Kahve AI pipeline

- [ ] İlk Qwen aşaması: yalnız structured visual analysis.
- [ ] `region`, `shape`, `rawConfidence`, view bilgisi modelini tanımla.
- [ ] 2–3 fotoğrafı tek `merged cup analysis` altında birleştir.
- [ ] Duplicate sembol deduplication.
- [ ] Çelişkili bulguları uncertain/eleme.
- [ ] Confidence self-score'u kör kullanmama.
- [ ] İkinci Qwen aşaması: yalnız merged analysis üzerinden fal metni.
- [ ] Genel / Aşk / İş-Para / Yol-Değişim sonuç blokları.
- [ ] Ortak safety filter'dan geçir.
- [ ] Sahte ilerleme yüzdesi yerine gerçek state'ler göster.

**Bitiş kriteri:** Kahve sonucu gerçek structured image analysis'ten türetiliyor; tek promptla uydurma fal yok.

---

## FAZ 9 — Kahve doğruluk / regresyon QA

- [ ] En az 100 gerçek fincan fotoğrafı/grubu.
- [ ] İnsan etiketli görünür bölge/sembol ground truth.
- [ ] 2–3 fotoğraflı aynı fincan cross-view örnekleri.
- [ ] Sembol bulunmayan negatif örnekler.
- [ ] Zor ışık/bulanık/yanıltıcı desen örnekleri.
- [ ] Confidence calibration.
- [ ] Hedef high-confidence precision ≥ %80.
- [ ] Hedef high-confidence false-positive ≤ %15.
- [ ] Aynı görüntü tekrarında ciddi sembol sıçraması testi.

**Bitiş kriteri:** Kahve kalite hedefleri sağlanmadan sonraki release aşamasına hazır sayılmaz.

---

## FAZ 10 — Tarot engine + UI

- [ ] 78 kart metadata + asset mapping.
- [ ] Türkçe kart adları standardı.
- [ ] `Random.secure()` veya eşdeğeri draw engine.
- [ ] Aynı açılımda duplicate kart engeli.
- [ ] Upright/reversed state mantığını merkezi config'e al.
- [ ] Tek Kart.
- [ ] 3 Kart: Geçmiş / Şimdi / Gelecek.
- [ ] 5 Kart: Geçmiş / Şimdi / Gizli Etki / Yakın Gelecek / Sonuç-Tema.
- [ ] Kullanıcı sorusu opsiyonel.
- [ ] Deste karıştırma + kapalı kart seçme UX'i.
- [ ] Kart flip/selection animasyonlarını hafif tut.
- [ ] Qwen'e structured kart ID/ad/pozisyon/state gönder.
- [ ] Qwen'in listede olmayan kart uydurmasını engelle.
- [ ] Kart kart + kombinasyon + genel yorum üret.
- [ ] Ortak safety filter'dan geçir.
- [ ] 78 kart integrity otomatik testi.

**Bitiş kriteri:** 1/3/5 kart açılımı deterministik metadata üzerinden hatasız çalışıyor; AI kartı görselden tahmin etmiyor.

---

## FAZ 11 — El Falı fotoğraf kalite + privacy kontrolü

- [ ] 1–2 avuç içi fotoğraf desteği.
- [ ] Kullanıcıya doğru çekim rehberi: açık avuç, iyi ışık, minimum gölge.
- [ ] El/avuç varlık kontrolü.
- [ ] Blur/resolution/exposure/occlusion kontrolü.
- [ ] Avuç crop/normalize.
- [ ] Yanlış görselde retry.
- [ ] Ham avuç fotoğrafını varsayılan olarak inference sonrası sil.
- [ ] Parmak izi/biometric template üretmeyen data modelini doğrula.

**Bitiş kriteri:** El Falı yalnız yorum için gerekli geçici avuç görüntüsünü kullanıyor; kimlik/biometric sistemine dönüşmüyor.

---

## FAZ 12 — El Falı structured AI pipeline

- [ ] İlk Qwen aşaması yalnız görünür palm özelliklerini structured çıkarır.
- [ ] Aday alanlar:
  - kalp çizgisi,
  - baş çizgisi,
  - yaşam çizgisi,
  - kader çizgisi yalnız görünürse,
  - kesişim/dallanma/yoğunluk gibi görünür yapı.
- [ ] Görünmeyen çizgi için `not_visible/uncertain` state'i.
- [ ] Confidence ham sinyal olarak tutulur; validation ile kalibre edilir.
- [ ] İkinci aşama yalnız structured palm analysis üzerinden sembolik yorum üretir.
- [ ] `geleneksel el falında / sembolik olarak / çağrıştırabilir` dil kuralı.
- [ ] Deterministik kişilik hükmü engeli.
- [ ] Sağlık, yaşam süresi, hamilelik, hassas özellik tahmini engeli.
- [ ] Irk/etnik köken/din/siyasi görüş/cinsel yönelim çıkarımı engeli.
- [ ] Gereksiz yaş/cinsiyet tahmini engeli.
- [ ] Ortak safety filter'dan geçir.

**Bitiş kriteri:** El Falı yalnız görünür çizgileri sembolik/eğlence amaçlı yorumluyor; teşhis/tavsiye/biometric inference yok.

---

## FAZ 13 — El Falı doğruluk / bias / safety QA

- [ ] En az 100 gerçek avuç içi fotoğrafı/grubu.
- [ ] İnsan etiketli görünür ana çizgi/bölge ground truth.
- [ ] Farklı ten tonları, kamera kalitesi, ışık ve el pozisyonu çeşitliliği.
- [ ] Görünmeyen çizgi/negatif örnekler.
- [ ] False-positive ve confidence calibration.
- [ ] Görünmeyen çizgi uydurma testi.
- [ ] Hassas özellik çıkarımı jailbreak testi.
- [ ] Sağlık/ölüm/hamilelik/tavsiye ihlali regresyonu.

**Bitiş kriteri:** El Falı farklı görüntülerde tutarlı ve güvenli; yasak çıkarımlar üretmiyor.

---

## FAZ 14 — Ortak fal sonucu + Fal Sohbeti

- [ ] Ortak result shell/component.
- [ ] Zorunlu entertainment/professional-advice disclaimer.
- [ ] Kahve structured context bağla.
- [ ] Tarot metadata context bağla.
- [ ] El structured palm context bağla.
- [ ] Her fal için unique local conversation.
- [ ] Chat yalnız aktif fal bağlamında.
- [ ] Fal dışı scope rejection.
- [ ] Prompt injection guard.
- [ ] Context büyüyünce local summary.
- [ ] Chat yazıyor/error/retry state.
- [ ] Chat çıktısını da kesinlik/tavsiye safety filtresinden geçir.

**Bitiş kriteri:** Üç fal türü aynı kontrollü sonuç ve sohbet katmanını kullanıyor; genel chatbota dönüşmüyor.

---

## FAZ 15 — Geçmiş Fallar + local storage

- [ ] Local database/storage seç.
- [ ] Kahve/Tarot/El fal kayıt tipleri.
- [ ] Structured analysis + final text kaydı.
- [ ] Chat geçmişini fala bağla.
- [ ] Ham kahve/el fotoğraflarını varsayılan kalıcı kayda dahil etme.
- [ ] Son falı dashboard'da göster.
- [ ] Tek fal silme.
- [ ] Tüm geçmiş silme.
- [ ] App data reset.

**Bitiş kriteri:** Üç fal türünün geçmişi server olmadan güvenli yönetiliyor ve tamamen silinebiliyor.

---

## FAZ 16 — Reklam monetizasyonu

Ayrıntı kaynağı: `MONETIZATION_V1.md`.

- [ ] Google AdMob + UMP.
- [ ] Merkezi `MonetizationConfig/AdPolicy`.
- [ ] `rewardedAdsPerUnlock = 2`.
- [ ] Kahve full result: 2 Rewarded.
- [ ] Tarot full result: 2 Rewarded.
- [ ] El Falı full result: 2 Rewarded.
- [ ] Chat message pack: 2 Rewarded.
- [ ] `0/2 → 1/2 → 2/2` transaction state.
- [ ] Her reklam ayrı kullanıcı opt-in.
- [ ] Tek reklamda entitlement verme.
- [ ] `timedInterstitialEligibilitySeconds = 90`.
- [ ] 90 sn yalnız eligibility; doğal geçişte interstitial.
- [ ] Fotoğraf/AI/result reading/card-palm selection/chat/rewarded sırasında timed interstitial yok.
- [ ] Telefon anchored adaptive banner.
- [ ] Tablet/BlueStacks güvenli tek side rail opsiyonu.
- [ ] Chat/analiz/çekim/seçim ekranında banner yok.
- [ ] App Open çakışma kontrolleri.
- [ ] Reklam load fail/retry; entitlement bypass yok.
- [ ] Development yalnız test ad unit ID.

**Bitiş kriteri:** Ücretsiz kullanıcıdaki bütün reklam akışları merkezi ve testli; yanlış tıklama, dead-end veya ad-bypass yok.

---

## FAZ 17 — Premium aylık reklamsız abonelik

- [ ] Google Play Billing güncel entegrasyonu.
- [ ] Tek ürün: aylık reklamsız Premium.
- [ ] Fiyat/dönem/otomatik yenileme/iptal açıklaması.
- [ ] Purchase + pending + restore.
- [ ] Renewal/grace/account hold/expiry/cancel.
- [ ] Process-death recovery.
- [ ] Entitlement cache/state.
- [ ] Premium'da Rewarded/timed/App Open/banner/native tamamen kapalı.
- [ ] Premium'da Kahve/Tarot/El/chat doğrudan kullanılıyor.
- [ ] Subscription management link.

**Bitiş kriteri:** Premium aktif olduğunda uygulamada hiçbir reklam request/container görünmüyor; lifecycle doğru.

---

## FAZ 18 — Privacy, AI safety, Play policy ve raporlama

- [ ] Privacy Policy.
- [ ] Data Safety.
- [ ] Açık kaynak lisans ekranı + Qwen attribution/NOTICE.
- [ ] Asset lisans listesi.
- [ ] AI output in-app report/flag akışı.
- [ ] Report endpoint minimum payload ve privacy kontrolü.
- [ ] Ham kahve/el fotoğrafını report payload'a varsayılan ekleme.
- [ ] Kullanıcının önemli hayat kararına emir/tavsiye veren AI çıktılarını regresyonla engelle.
- [ ] Sağlık/hukuk/finans/hamilelik/ölüm kesinliği testi.
- [ ] El Falı biometric/hassas trait yasağı testi.
- [ ] Production sensitive logging kapalı.
- [ ] Secrets scan.
- [ ] Store listing'in eğlence amaçlı AI fal niteliğini yanıltmadan açıklaması.

**Bitiş kriteri:** Bilinen Play/privacy/lisans/safety bloklayıcısı yok ve üç fal türü ortak güvenlik standardına uyuyor.

---

## FAZ 19 — Release model dağıtımı

- [ ] Release tarihindeki Google Play on-device model/asset delivery yöntemini yeniden doğrula.
- [ ] Modeli base APK içine gömme.
- [ ] Model availability + device RAM/ABI/disk check.
- [ ] Download progress/cancel/retry.
- [ ] SHA-256/version doğrulama.
- [ ] Model update sonrası eski sürüm cleanup.
- [ ] `bundletool` local testing.
- [ ] Internal Play test track.
- [ ] Debug local model ve release model aynı `ModelManager` interface'i.

**Bitiş kriteri:** Model dağıtımı production senaryosunda doğrulanmış; inference model indikten sonra offline.

---

## FAZ 20 — Performans + responsive + stress QA

- [ ] 4 GB gerçek cihaz.
- [ ] 6 GB gerçek cihaz.
- [ ] 8 GB+ gerçek cihaz.
- [ ] Tablet.
- [ ] BlueStacks.
- [ ] App cold start.
- [ ] Qwen warm-up.
- [ ] First token.
- [ ] Kahve full analysis süresi.
- [ ] El full analysis süresi.
- [ ] Peak RAM.
- [ ] 20+ ardışık karma fal stress testi.
- [ ] Uzun chat memory leak.
- [ ] Background/foreground.
- [ ] Low-memory kill/recovery.
- [ ] Inference cancel.
- [ ] Thermal uzun kullanım.
- [ ] TalkBack + font scaling.
- [ ] Banner tablet/telefon layout QA.

**Bitiş kriteri:** Crash/OOM/bloklayan UX yok; üç fal türü ve reklam UI bütün hedef ekranlarda responsive.

---

## FAZ 21 — Ağ izolasyonu + gizlilik kanıtı

- [ ] Kahve inference network inspection.
- [ ] El inference network inspection.
- [ ] Tarot/chat network inspection.
- [ ] Fotoğraf/prompt/chat outbound inference request olmadığını doğrula.
- [ ] Temp photo cleanup testi.
- [ ] Auto Backup exclusion testi.
- [ ] Production log leak testi.
- [ ] Yalnız izin verilen AdMob/Play/Billing/user-triggered report ağlarını doğrula.

**Bitiş kriteri:** AI fotoğraf/prompt/chat inference verisinin cihazdan çıkmadığı kanıtlandı.

---

## FAZ 22 — Final regresyon + teslim

- [ ] `SPECIFICATION.md` checklist.
- [ ] `V1_RELEASE_GATES.md` checklist.
- [ ] `MONETIZATION_V1.md` checklist.
- [ ] Tüm unit/widget/integration testleri.
- [ ] `flutter analyze` bloklayıcı hata yok.
- [ ] Debug APK clean build.
- [ ] Signed release APK clean build.
- [ ] Signed release AAB clean build.
- [ ] Clean install/open.
- [ ] Upgrade/uninstall/reinstall.
- [ ] Model missing/corrupt/reinstall.
- [ ] AdMob test → production ID kontrolü.
- [ ] Billing test → production product ID kontrolü.
- [ ] VersionCode/VersionName.
- [ ] Store listing + privacy URL.
- [ ] Final commit/tag.
- [ ] APK + AAB + test raporu + commit SHA kaydı.

**Bitiş kriteri:** Açık checklist maddesi yok; APK/AAB temiz kuruluyor ve Kahve + Tarot + El Falı uçtan uca gerçek cihaz testini geçiyor.

---

# BLOKLAYICI OLMAZSA OLMAZLAR

1. LP FAL adı ve mevcut Mizan/Lefferion Prime logosu.
2. Açık/krem onaylı dashboard.
3. Kahve + Tarot + El Falı V1'de tamam.
4. Dekoratif uygulama görselleri lisanslı ve AI üretimi değil.
5. Qwen lokal; AI inference verisi sunucuya gitmiyor.
6. Kahve merged structured analysis kullanıyor.
7. Tarot 78 kart engine/mapping sağlam.
8. El Falı yalnız görünür avuç çizgilerini sembolik yorumluyor.
9. El Falı biometric kimlik/parmak izi template'i oluşturmuyor.
10. Kesin gelecek ve hayat kararı yönlendiren tavsiye yok.
11. Sağlık/ölüm/hamilelik/hukuk/garantili finans kesinliği yok.
12. Kahve + El confidence validation ile kalibre.
13. Chat yalnız aktif fal bağlamında.
14. Her free unlock 2 Rewarded Ad.
15. Timed interstitial eligibility 90 saniye + doğal geçiş.
16. Telefon/tablet banner yerleşimi güvenli.
17. Premium'da hiçbir reklam yok.
18. Geçmiş yerelde ve silinebilir.
19. 4/6/8 GB + tablet/BlueStacks QA.
20. Privacy/Data Safety/AI reporting/lisans tamam.
21. Signed APK + AAB temiz testten geçmiş.

**Test kanıtı olmayan checkbox tamamlanmış sayılmaz.**