# LP FAL — UI Asset Manifest / Görsel Repo Kontrolü

**Kontrol tarihi:** 2026-09-15  
**Durum:** Repo `main` üzerinde doğrulandı.

Bu dosya üretilmiş UI görsellerinin repoda gerçekten bulunduğunu kayıt altına alır ve runtime / tasarım referansı ayrımını bağlayıcı hale getirir.

## 1. Full-screen / tasarım referansları — `design_refs/ui/`

Repo içinde **12 tasarım referansı** vardır:

- `analysis_screen_concept.jpg`
- `chat_screen_concept_v1.jpg`
- `chat_screen_concept_v2.jpg`
- `coffee_capture_screen_concept.jpg`
- `dashboard_screen_concept.jpg`
- `dream_interpretation_screen_concept.jpg`
- `palm_capture_screen_concept.jpg`
- `premium_screen_concept.jpg`
- `tarot_screen_concept.jpg`
- `history_screen_concept.jpg` — thumbnail içermeyen güncel Geçmiş tasarımı.
- `icon_usage_showcase_concept.jpg` — kategori ikonlarının Dashboard/Geçmiş/kart alanlarında nasıl kullanılacağına dair referans.
- `icon_system_style_guide_concept.jpg` — dört ana modül ikonunun uygulama genelindeki görsel dilini ve örnek kullanımını gösteren son onaylı stil rehberi; gerçek JPEG binary blob `1ef181a908f28d37862fbf7675240951a0b2ad24`.

Bu dosyalar yalnız tasarım referansıdır; runtime'da tam ekran screenshot olarak kullanılmaz.

## 2. Uygulama içi runtime görsel asset seti — `assets/ui/`

Repo içinde **17 runtime UI asset'i** vardır.

### Background
- `assets/ui/backgrounds/dashboard_background_main.jpg`

### Mode cards
- `assets/ui/cards/coffee_fortune_card.jpg`
- `assets/ui/cards/tarot_fortune_card.jpg`
- `assets/ui/cards/palm_fortune_card.jpg`
- `assets/ui/cards/dream_fortune_card.jpg` — Rüya Tabiri dashboard kartı.

### Mode icons — transparent PNG
- `assets/ui/icons/coffee_fortune_icon.png`
- `assets/ui/icons/tarot_fortune_icon.png`
- `assets/ui/icons/dream_interpretation_icon.png`
- `assets/ui/icons/palm_fortune_icon.png`

Kategori ikonları 192×192 şeffaf PNG çalışma asset'idir. Flutter tarafında sabit fiziksel piksel boyutuyla değil responsive logical size / constraints / `BoxFit.contain` ile kullanılacaktır. Önerilen kullanım aralıkları: liste 24–28dp, kart 28–36dp, başlık 32–40dp; tablet/BlueStacks'ta layout constraint'e göre ölçeklenir.

### Heroes
- `assets/ui/heroes/coffee_hero_screen.jpg`
- `assets/ui/heroes/tarot_hero_screen.jpg`
- `assets/ui/heroes/palm_hero_screen.jpg`
- `assets/ui/heroes/dream_hero_screen.jpg`

### Other runtime visuals
- `assets/ui/chat/fortune_chat_banner.jpg`
- `assets/ui/results/fortune_result_hero.jpg`
- `assets/ui/splash/lpfal_splash.jpg`
- `assets/ui/states/ai_analysis.jpg`

## 3. Toplam doğrulanan UI görsel dosyası

- Tasarım referansı: **12**
- Runtime UI asset'i: **17**
- Toplam: **29 doğrulanmış UI görsel dosyası**

Son kaydedilen onaylı referans: `icon_system_style_guide_concept.jpg`.

## 4. Geçmiş ekranı kuralı

Geçmiş kayıt listelerinde fal/rüya için üretilmiş thumbnail illüstrasyonu kullanılmaz. Kayıt satırında yalnız:
- ilgili kategori ikonu,
- tür/başlık,
- tarih/saat,
- kısa metin,
- açma/chevron aksiyonu
bulunur.

## 5. Premium ve restore görünürlüğü

- Kullanıcıya görünen ürün adı yalnız `LP FAL Premium` / `Premium`.
- `PRO/Pro`, abonelik veya `Ömür Boyu/Lifetime` paket adı yok.
- Restore otomatik ve sessizdir.
- Kullanıcıya restore butonu veya restore metni gösterilmez.

## 6. Legacy mockup politikası

Eski mockup'ta yanlış marka, eski plan adı, görünür restore kontrolü, geçmiş thumbnail'ı veya V1 dışı özellik görülmesi runtime gereksinimi değildir. Yeni şartname ve güncel referanslar önceliklidir.

## 7. Release asset gate

- [ ] Runtime asset yolları `pubspec.yaml` içinde yalnız gerektiği kadar tanımlı.
- [ ] Missing asset yok.
- [ ] 4 kategori ikonu şeffaf ve responsive kullanılıyor.
- [ ] Geçmiş listelerinde thumbnail yok.
- [ ] Premium UI'da görünür restore kontrolü yok.
- [ ] Runtime UI'da `PRO/Pro` veya abonelik/süre çağrıştıran plan adı yok.
- [ ] Tasarım JPG'leri tam ekran statik UI olarak kullanılmıyor.
- [ ] Harici/public-domain asset varsa lisans/source manifesti tamam.

**Bu manifest görsel dosya varlığı ve görsel-copy QA için zorunlu kontroldür.**