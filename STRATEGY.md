# STRATEGY.md — Mobil Uygulama Portföyü: İş Stratejisi ve Süreç

> Bu dosya `AndroidStudioProjects` altındaki TÜM uygulama projelerinde geçerlidir ve CLAUDE.md'deki
> davranış kurallarıyla birlikte uygulanır. Mağaza politikaları/ücretleri **2026-07-12** tarihinde
> web üzerinden doğrulanmıştır — kritik kararlardan (yayın, fiyatlandırma) önce güncelliği yeniden doğrula.

## İş Modeli

- Amaç: **Kâr amaçlı**, çapraz platform (Android + iOS) uygulama **portföyü**. Uzun vadeli gelir hedefi.
- Her uygulama hem Google Play hem Apple App Store'da yayınlanır.
- Claude sadece kod değil; pazar araştırması, yayınlama ve pazarlama dahil sürecin tamamında rehberlik eder.

## Teknoloji Yığını (tüm projelerde varsayılan)

- **React Native + Expo** (güncel SDK, New Architecture) + **TypeScript** (strict)
- Navigasyon: **expo-router**
- Build & yayın: **EAS Build + EAS Submit** (Windows'tan iOS dahil — Mac gerekmez, cloud build)
- OTA güncelleme: **EAS Update** (JS/asset düzeltmeleri mağaza incelemesi olmadan)
- Monetizasyon: **RevenueCat** (`react-native-purchases`); reklam gerekirse `react-native-google-mobile-ads` (Invertase)
- Bildirim: `expo-notifications`; Analitik: PostHog veya Firebase Analytics (uygulamaya göre seç)
- State: yeterli olan en basiti (varsayılan Zustand; global state gerekmiyorsa local state/Context)
- Mimari: feature-bazlı klasör yapısı; CLAUDE.md'nin sadelik ilkeleri geçerli

## Uygulama Başına Zorunlu İş Akışı

**Faz 0 — Fikir:** Kullanıcı uygulama fikrini paylaşır.

**Faz 1 — Pazar Araştırması (Claude, kod yazmadan ÖNCE):**
- Her iki mağazada rakip analizi: özellikler, fiyatlandırma, indirme/yorum sinyalleri, monetizasyon modelleri
- Bizim uygulamanın farkları, üstün ve eksik yanları
- **Spam-riski değerlendirmesi:** kategori doygun mu? Apple 4.3(b) / Google "repetitive content" açısından
  "anlamlı farklılaşma" var mı? (aşağıdaki Mağaza Gerçekleri bölümüne bak)
- Başarı olasılığı değerlendirmesi + net **GO / NO-GO önerisi** ve gerekçesi

**Faz 2 — Onay kapısı:** Kullanıcı onaylamadan geliştirmeye başlanmaz.

**Faz 3 — Geliştirme:** Mimari plan → onay → implementasyon. CLAUDE.md kuralları geçerli;
her özellik doğrulanabilir başarı kriteriyle tanımlanır.

**Faz 4 — Test:** Android lokal cihaz/emülatör; iOS fiziksel iPhone (Expo Go → development build → TestFlight).
**Play kapalı testi (closed testing) mümkün olan en erken anda başlat** — 14 günlük sayaç erken işlesin.

**Faz 5 — Yayınlama:**
- Store varlıkları: ikon, ekran görüntüleri (ilk görüntü en kritik), açıklama, anahtar kelimeler
- Gizlilik politikası URL'i + Apple privacy labels + Google Data Safety formu (zorunlu)
- **iOS-first lansman:** Apple'da tester zorunluluğu yok → iOS önce yayınlanır;
  Android kapalı test süreci paralel yürür, bitince production başvurusu yapılır

**Faz 6 — Pazarlama & ASO:** Aşağıdaki playbook'un uygulama başına asgari maddeleri tamamlanır.

**Faz 7 — Lansman sonrası:** Metrik takibi ve karar kriterleri (aşağıda) uygulanır.

## Mağaza Gerçekleri (doğrulandı: 2026-07-12)

| Konu | Google Play | Apple App Store |
|---|---|---|
| Hesap ücreti | $25 tek seferlik | $99/yıl |
| Yeni hesap zorunluluğu | **Her uygulama için kapalı test: 12 tester × 14 kesintisiz gün** + production başvurusu (~7 gün) → **uygulama başına min. ~3 hafta** | Yok (TestFlight opsiyonel) |
| İnceleme süresi | Yeni hesaplarda ~7-14 gün (ilk uygulama); sonra 1-3 gün | %90'ı <24 saat (resmi); yeni hesaplarda 1-7 gün görülebiliyor |
| Komisyon | %15 (ilk $1M; 30 Haz 2026'dan itibaren ABD/AEA/UK: %10 servis + %5 Play Billing) | Small Business Program: **%15** (<$1M/yıl; yeni geliştirici otomatik uygun) |
| Bireysel hesap notu | — | Mağazada satıcı olarak **yasal adınız** görünür; marka adı için şirket + D-U-N-S gerekir |

### ⚠️ Spam Politikaları (İŞ MODELİNİ ŞEKİLLENDİREN KISIT)

- **Google "Repetitive Content":** "Birbirine çok benzeyen işlevsellik, içerik ve deneyime sahip birden çok
  uygulama oluşturmak" ihlaldir → uygulama kaldırma ve **hesap kapatmaya** kadar gider.
- **Apple 4.3(b)** (Haziran 2026'da sıkılaştırıldı): Doygun kategorilerde (duvar kağıdı, ses efekti,
  basit zamanlayıcı, fal, el feneri vb.) "anlamlı derecede farklı veya iyileştirilmiş deneyim" sunmayan
  uygulamalar reddedilir; mevcut olanlar **geriye dönük kaldırılabilir**; tekrarlanan düşük-efor
  gönderimler **Developer Program'dan atılmayla** sonuçlanabilir. Apple, uygulamalar arası ortak
  kod/asset'leri otomatik tespit ediyor.
- **Sonuç:** Şablon/reskin "app factory" modeli 2026'da uygulanamaz. Her uygulama gerçek ve
  savunulabilir bir farklılaşma taşımak zorunda. Faz 1'deki spam-riski değerlendirmesi bu yüzden zorunlu.

## Tempo ve Pipeline

- "Haftada 2 uygulama yayında" hedefi Play'in uygulama başına ~3 haftalık zorunlu test süreci nedeniyle
  takvimsel olarak mümkün değil; ayrıca spam politikaları nedeniyle stratejik olarak riskli.
- **Çalışma modeli: boru hattı (pipeline)** — aynı anda 2-3 uygulama farklı fazlarda ilerler
  (biri geliştirmede, biri kapalı testte, biri pazarlamada).
- **Hedef tempo: ~2 haftada 1 yeni kaliteli uygulama** + kazanan uygulamalara iterasyon.
  Gerekçe: abonelik gelirinin %94.5'i en iyi %10 uygulamada toplanıyor — adet değil kalite kazandırıyor.

## Monetizasyon Varsayılanları (veri: RevenueCat & Adapty 2026 raporları)

- **Varsayılan model: abonelik + lifetime (hibrit), hard paywall.**
  Hard paywall freemium'a göre ~5x iyi dönüşüyor (D35 %10.7 vs %2.1; kişi başı gelir $3.09 vs $0.38).
- Fiyat bantları (2026 piyasa normu): **$4.99-6.99/hafta, $7.99-9.99/ay, $29.99-39.99/yıl**;
  lifetime ≈ yıllığın 2-3 katı.
- Reklam tek başına utility uygulamalarında yetersiz (anlamlı gelir ~50K DAU ister);
  varsa "reklamları kaldır" IAP'i olarak konumlandır.
- RevenueCat ile paywall/fiyat A/B testleri: ayda en az 1 deney (test eden uygulamalar 18.7x fark yaratıyor).
- **Gerçekçi beklenti:** abonelikli uygulamaların sadece %17.3'ü $1K/ay'a ulaşıyor; %57.7'si toplamda
  $1K'ı hiç göremiyor. Plan tek uygulamaya değil, portföye ve hızlı karar vermeye dayanır.

## Pazarlama Playbook (uygulama başına asgari)

1. **ASO:** ana anahtar kelime başlıkta (30 kr), ikincil alt başlıkta; iOS 100 karakterlik keyword alanı
   doldurulur. İlk ekran görüntüsü belirleyici (kullanıcı ~7 sn'de karar verir; Apple ekran görüntüsü
   metinlerini OCR ile indeksliyor — caption'lara anahtar kelime koy).
2. **Lokalizasyon:** EN + TR baştan; metadata/caption bazında ES, DE, PT, FR eklenir
   (lokalize listing 2-3x indirme; UI İngilizce kalabilir — 80/20 kuralı).
3. **Landing page + web2app hunisi:** basit web sayfası; ABD'de harici ödeme linki şu an komisyonsuz
   (hukuken değişken — kullanmadan önce doğrula).
4. **Kanal seçimi:** 1 algoritmik kanal (TikTok/Reels/Shorts) + 1 topluluk kanalı (Reddit/niş forum),
   derinlemesine ve düzenli. Beş kanala yüzeysel yayılma.
5. **İlk ücretli kanal:** küçük bütçeli Apple Search Ads (yüksek niyetli trafik).
6. **Lansman:** uygunsa Product Hunt / Show HN; App Store featuring başvurusu (App Store Connect formu).
7. **Yorum toplama:** başarı anlarında in-app review prompt (SKStoreReviewController / In-App Review API).

## Lansman Sonrası Karar Kriterleri

- $1K/ay'a ulaşan uygulamalar bunu tipik olarak ilk ~60 günde başarıyor → **60 gün değerlendirme penceresi.**
- 60 günde sinyal yok (ör. <$100/ay ve büyümüyor): bakım moduna al, kaynakları sıradaki uygulamaya kaydır.
- Sinyal var: iterasyon, paywall deneyleri ve pazarlama bütçesi bu uygulamaya yoğunlaşır.
- Faturalandırma hatası kaynaklı istem dışı churn'ü izle (Play iptallerinin %31'i) — grace period aç.

## Sabit Maliyetler

| Kalem | Maliyet |
|---|---|
| Google Play Console (bireysel, tek sefer) | $25 |
| Apple Developer Program (yıllık) | $99 |
| Expo EAS | $0 (ayda 15 iOS + 15 Android build) → $19/ay Starter (büyüyünce) |
| RevenueCat | Ücretsiz başlangıç katmanı |
| Fiziksel iPhone (iOS testi için gerekli — simülatör Mac-only) | mevcut değilse edinilmeli |

## Riskler ve Notlar

- Play'de **organizasyon (şirket) hesabı 12-tester zorunluluğundan muaf** — hacim artarsa şirketleşme
  + D-U-N-S ile hem bu kısıt kalkar hem Apple'da marka adı kullanılabilir.
- EAS Update MAU limiti hesap genelidir (tüm uygulamaların toplamı) — portföy büyüdükçe Starter'a geçilir.
- Mağaza politikaları ve komisyonlar 2026'da geçiş halinde (davalar/uzlaşmalar) — yayın öncesi yeniden doğrula.
- Google Play kapalı testinde tester'lar 14 gün boyunca **aktif kalmalı** (sadece kayıt yetmez;
  düşük engagement ret sebebi) — güvenilir 12+ kişilik tester ağı kur ve koru.
- **12 tester bulunamazsa:** Play'de production teknik olarak kilitlidir (yayın imkânsız, ceza değil).
  Fallback sırası: (1) karşılıklı test toplulukları (r/AndroidClosedTesting, Discord grupları) —
  ücretsiz ve meşru; (2) ücretli tester servisi (~$20-70, düşük-engagement ret riski var);
  (3) bu sürede uygulama **iOS-only yayınlanır** ve gelir üretmeye başlar (kullanıcının iPhone'u var,
  Apple'da tester şartı yok) — Android, test şartı sağlanınca eklenir.
