# LP FAL — UI Asset Manifest / Görsel Repo Kontrolü

**Kontrol tarihi:** 2026-09-10  
**Durum:** Repo `main` üzerinde doğrulandı.

Bu dosya üretilmiş UI görsellerinin repoda gerçekten bulunduğunu kayıt altına alır ve Premium metin standardını görsel katmana bağlar.

## 1. Full-screen tasarım referansları — `design_refs/ui/`

Aşağıdaki **8 JPG** repo içinde mevcut:

| Dosya | Blob SHA | Boyut |
|---|---|---:|
| `design_refs/ui/analysis_screen_concept.jpg` | `6f55d8c2d01f0fcb00b109e99e47219c47b9a148` | 8,141 B |
| `design_refs/ui/chat_screen_concept_v1.jpg` | `d17892bc02f1c88ad3d2a62eeb6febc4634cc254` | 7,537 B |
| `design_refs/ui/chat_screen_concept_v2.jpg` | `6c4523de1b25cfdbbd9a32c00c32cb71ae81c8aa` | 7,573 B |
| `design_refs/ui/coffee_capture_screen_concept.jpg` | `1dc3589c283ea90e0453dc3dad7d4be45ba71f71` | 8,000 B |
| `design_refs/ui/dashboard_screen_concept.jpg` | `c489183738735f7e850764bf92e5c532ae2e9c83` | 7,723 B |
| `design_refs/ui/dream_interpretation_screen_concept.jpg` | `6be2fac5394c1c27b8bc0bfb0088716c308160e1` | 10,628 B |
| `design_refs/ui/palm_capture_screen_concept.jpg` | `bf872f44d6bf1e043244a5b0c0f4bab7a7b1fb22` | 8,358 B |
| `design_refs/ui/tarot_screen_concept.jpg` | `de24d0a632f086faa41f0fec701bfee280ebca72` | 7,602 B |

Bu dosyalar **yalnız tasarım referansıdır**. Runtime'da tam ekran screenshot olarak kullanılmaz.

## 2. Uygulama içi görsel asset seti — `assets/ui/`

Aşağıdaki **11 JPG** repo içinde mevcut:

| Dosya | Blob SHA | Boyut |
|---|---|---:|
| `assets/ui/backgrounds/dashboard_background_main.jpg` | `07693171ad069cf19222a84f6e5f7ce1dcd952f4` | 5,192 B |
| `assets/ui/cards/coffee_fortune_card.jpg` | `e88980f0fe94f67449f055caeadfafdffbd8a463` | 4,780 B |
| `assets/ui/cards/palm_fortune_card.jpg` | `f9e56a44025973e752ec93e153046d148e22d511` | 5,454 B |
| `assets/ui/cards/tarot_fortune_card.jpg` | `998196ab736bfead05f6496e735084b1ccd1b6e6` | 5,235 B |
| `assets/ui/chat/fortune_chat_banner.jpg` | `7456ae6a99a4797831c764515c8d1807459b9be4` | 13,287 B |
| `assets/ui/heroes/coffee_hero_screen.jpg` | `d2cf5376749bbe33627394f174c3fc494c1018b4` | 11,374 B |
| `assets/ui/heroes/palm_hero_screen.jpg` | `62f1976d43f5c52559cbdf24c2fcc89a51cb10be` | 4,623 B |
| `assets/ui/heroes/tarot_hero_screen.jpg` | `ee245eb1ac5f405e1a98889c24c806ab509874d8` | 9,118 B |
| `assets/ui/results/fortune_result_hero.jpg` | `60e9bd6b4a96910ae0e7709737cac31c8ad3d4e2` | 11,448 B |
| `assets/ui/splash/lpfal_splash.jpg` | `4e767f1e545e0c66fc6e21950d415e99baf35c46` | 10,894 B |
| `assets/ui/states/ai_analysis.jpg` | `886a934e356d16a1bf728d9f892389a84e3b0687` | 6,378 B |

## 3. Toplam doğrulanan görseller

- Tasarım referansı: **8**
- `assets/ui` görseli: **11**
- Toplam repo içinde doğrulanan JPG: **19**

Son onaylanan Rüya Tabiri ekranı `dream_interpretation_screen_concept.jpg` olarak kaydedildi. Önceki Rüya taslağı değil, kullanıcının onayladığı; geçmiş rüyalarda thumbnail kullanmayan ve `Duygu Ekle` akışını gösteren sürüm referanstır.

## 4. Geçersiz / yeniden üretilecek mockup politikası

Aşağıdaki eski tasarım yönleri geçerli referans sayılmaz ve mevcut UI standardını değiştiremez:
- eski Premium ekranındaki abonelik/çoklu plan/legacy plan etiketi,
- eski Geçmiş ekranındaki kayıt thumbnail görselleri,
- eski Rüya ekranındaki geçmiş rüya thumbnail görselleri,
- `Kozmik Falın` gibi LP FAL dışı geçici marka metinleri.

Bunlar yeniden üretildiğinde yalnız yeni, şartnameye uygun sürüm repo referansı yapılır.

## 5. Premium copy kuralı

Premium ürün/özellik adında tek kullanıcı kelimesi **Premium**'dur.

Kullanılacak:
- `Premium`
- `LP FAL Premium`

Satın alma açıklaması:
- `Tek seferlik satın alım · Abonelik değildir.`

## 6. JPG içi legacy metin politikası

`design_refs/ui/*.jpg` dosyaları raster/JPG olduğundan içlerindeki yazı runtime copy kaynağı değildir. Eski görselde legacy plan etiketi görülürse uygulamaya taşınmaz. Flutter widget copy'si şartnamedeki güncel metin olur ve yeni/yenilenen mockup yalnız güncel standardı kullanır.

## 7. Release asset gate

- [ ] Yukarıdaki dosyalar repo içinde mevcut.
- [ ] Flutter `pubspec.yaml` yalnız gerçekten kullanılan runtime asset'leri içeriyor.
- [ ] Missing asset yok.
- [ ] License/source manifest tamam.
- [ ] Runtime UI'da Premium için eski plan etiketi yok.
- [ ] Runtime UI'da süre/abonelik çağrıştıran plan adı yok.
- [ ] Premium plan adı `LP FAL Premium`.
- [ ] Geçmiş kayıt listelerinde fal/rüya thumbnail görseli yok.
- [ ] Tasarım JPG'leri tam ekran statik UI olarak kullanılmıyor.

**Bu manifest görsel dosya varlığı ve görsel-copy QA için zorunlu kontroldür.**