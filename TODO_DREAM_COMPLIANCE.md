# LP FAL — Rüya Tabiri + Compliance Uygulama TODO Eki

**Durum:** ZORUNLU / normatif  
**Bağlı:** `TODO.md`, `DREAM_INTERPRETATION_SPEC.md`, `COMPLIANCE_BY_DESIGN.md`

Bu dosya ana `TODO.md` sırasını yeniden numaralandırmadan Rüya Tabiri ve uygulama-geneli compliance görevlerini ekler.

---

## FAZ 1C — Compliance temeli (ana Safety fazıyla birlikte)
- [ ] `CompliancePolicy` / `SafetyPolicy` merkezi arayüzü.
- [ ] Global disclaimer component.
- [ ] AI output `safe / rewrite / highRisk` state modeli.
- [ ] In-app AI report/flag veri modeli.
- [ ] Production log redaction standardı.
- [ ] Store/listing misleading-claim checklist'i.
- [ ] KVKK veri envanteri şablonu: veri, amaç, saklama, aktarım, hukuki sebep, silme.

**Bitiş:** Compliance yalnız prompt metni değil, ayrı uygulama katmanı olarak mimaride mevcut.

---

## FAZ 3D — Dashboard Rüya kartı
- [ ] Dashboard'a **Rüya Tabiri** kartı ekle.
- [ ] CTA: `Rüyanı Anlat` / `Rüyanı Yorumla`.
- [ ] Telefon/tablet/BlueStacks dört ana modülle responsive grid.
- [ ] Mockup'taki kapsam dışı modüller route'a dönüşmesin.

**Bitiş:** Kahve / Tarot / Rüya / El dört ana modül dashboard'dan gerçek route ile açılıyor.

---

## FAZ 10D — Rüya Tabiri engine + UI
Bu faz ana `TODO.md` içindeki Tarot fazından sonra, El Falı fazından önce uygulanır.

### Input/UI
- [ ] Gerçek multiline rüya input.
- [ ] Boş metinde CTA disabled.
- [ ] Opsiyonel duygu state'i.
- [ ] Input length/token sınırı merkezi config.
- [ ] Rüya metni production log/analytics'e yazılmasın.

### Structured extraction
- [ ] `DreamAnalysis` data model.
- [ ] `scenes`, `symbols`, `emotions`, `roles`, `uncertainItems` alanları.
- [ ] `dream_extract` prompt.
- [ ] Kullanıcının yazmadığı öğeleri eklememe guard/testi.

### Interpretation
- [ ] `dream_interpret` prompt.
- [ ] Kısa Özet.
- [ ] Öne Çıkan Semboller.
- [ ] Duygusal Atmosfer.
- [ ] Sembolik Temalar.
- [ ] Genel Sembolik Yorum.
- [ ] Belirsizlik/disclaimer.
- [ ] Tavsiye/öneri/karar bölümü YOK.

### Safety/compliance
- [ ] Kesin gelecek engeli.
- [ ] Sağlık/psikoloji teşhis engeli.
- [ ] Hukuk/finans yönlendirme engeli.
- [ ] Dini otorite/ilahi kesinlik engeli.
- [ ] Büyü/cin/lanet/paranoya doğrulama engeli.
- [ ] Third-party sensitive trait inference engeli.
- [ ] Self-harm/violence gerçek risk transition.

### Result/chat/history
- [ ] Dream result shell.
- [ ] In-app `Bildir` aksiyonu.
- [ ] `dream_chat` context.
- [ ] Yeni rüya = yeni conversation.
- [ ] Local history `type=dream`.
- [ ] Dream record delete + bağlı chat delete.

### Monetizasyon
- [ ] Free Dream full result = 2 Rewarded.
- [ ] İlk follow-up ücretsiz; chat pack = 2 Rewarded.
- [ ] Dream input/analysis/result/chat sırasında timed interstitial yok.
- [ ] Dream input/result/chat banner yok.
- [ ] Premium direct/no-ad.

**Bitiş:** Kullanıcı rüyasını yazıyor, AI yalnız gerçek inputtan structured öğeler çıkarıyor, tavsiye vermeden sembolik yorum üretiyor, sonucu kaydediyor ve aynı rüyayla sınırlı chat çalışıyor.

---

## FAZ 10DQ — Rüya kalite / safety QA
- [ ] En az 100 Türkçe Dream QA senaryosu.
- [ ] Normal/sembolik rüyalar.
- [ ] Çok kısa/belirsiz input.
- [ ] Kabus/ölüm.
- [ ] Hastalık/hamilelik.
- [ ] İlişki/ihanet.
- [ ] Finans/bahis.
- [ ] Dava/suç.
- [ ] Dini/doğaüstü kesinlik.
- [ ] Büyü/nazar/paranoya.
- [ ] Self-harm/violence gerçek risk.
- [ ] Prompt injection/jailbreak.
- [ ] `Kesin söyle`.
- [ ] `Ne yapmalıyım`.
- [ ] Rüyada olmayan ayrıntı uydurma.

**Bitiş:** Bloklayıcı compliance ihlali yok; groundedness ve safety regresyonu geçiyor.

---

## FAZ 14C — Ortak sonuç/chat compliance
- [ ] Kahve/Tarot/Rüya/El ortak disclaimer.
- [ ] Dört modül chat output compliance filter.
- [ ] AI mesajı başına/mesaj menüsünden report/flag.
- [ ] Report minimum payload.
- [ ] High-risk response state.
- [ ] General assistant scope rejection.

**Bitiş:** Dört modül aynı global compliance katmanını kullanıyor.

---

## FAZ 15D — Geçmiş Rüyalar
- [ ] Geçmiş listesinde Dream type.
- [ ] Rüya başlığı/tarih/özet.
- [ ] Dream result'a reopen.
- [ ] Dream chat'e reopen.
- [ ] Delete cascades to bağlı chat.
- [ ] Hassas text analytics/log'a gitmiyor.

---

## FAZ 16C — Monetizasyon compliance
- [ ] Dream reward transaction merkezi engine'i kullanıyor.
- [ ] Sensitive dream/fal text ad targeting'e gitmiyor.
- [ ] Ad system notification taklit etmiyor.
- [ ] Yanlış tıklama/dark pattern testi.
- [ ] Premium'da dört modül + tüm chat sıfır ad request.

---

## FİNAL-C — Hukuka/politikalara uygunluk release gate
- [ ] `COMPLIANCE_BY_DESIGN.md` 23 bölüm kontrol edildi.
- [ ] KVKK veri envanteri gerçek SDK/network davranışıyla çıkarıldı.
- [ ] Aydınlatma/Privacy Policy güncel veri akışıyla uyumlu.
- [ ] Play Data Safety gerçek SDK davranışıyla uyumlu.
- [ ] Google Play AI-generated content report/flag çalışıyor.
- [ ] AI output red-team dört modülde geçti.
- [ ] Store listing/screenshot yanıltıcı vaat içermiyor.
- [ ] Health declaration güncel Play gereksinimine göre doğru.
- [ ] Release tarihindeki Google Play policies yeniden doğrulandı.
- [ ] Release tarihindeki KVKK/mevzuat değişiklik kontrolü yapıldı.

**Bu blok geçmeden signed production AAB final kabul edilmez.**
