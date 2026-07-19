# metapsikoloji.tr — Kurulum Rehberi

Bu paketle gelen dosyalar:

| Dosya | Görevi |
|---|---|
| `index.html` | Ana landing page (tek sayfa) |
| `tesekkurler.html` | Form sonrası teşekkür sayfası — **form dönüşümü burada ölçülür** |
| `google-apps-script.js` | Form arka ucu: Sheets kaydı + anında e-posta bildirimi |
| `KURULUM.md` | Bu rehber |

Yasal sayfalar (`gizlilik-politikasi.html`, `kvkk-aydinlatma-metni.html`, `mesafeli-hizmet-sozlesmesi.html`) bir sonraki pakette gelecek; footer'daki linkleri şimdiden bu adlara göre hazırladım.

---

## Adım 1 — Dosyaları hostinge yükleyin

1. Hosting panelinizde (cPanel / Plesk vb.) **Dosya Yöneticisi**'ni açın.
2. `metapsikoloji.tr` alan adının kök klasörüne (`public_html` veya `httpdocs`) `index.html` ve `tesekkurler.html` dosyalarını yükleyin.
3. Kök klasörde `gorseller` adında boş bir klasör oluşturun (görseller geldikçe buraya yüklenecek).
4. SSL sertifikasının aktif olduğundan emin olun (`https://` ile açılmalı). Çoğu hostingde "Let's Encrypt" bir tıkla etkinleşir.

## Adım 2 — Form arka ucu (Google Sheets + e-posta)

1. [sheets.google.com](https://sheets.google.com) → boş bir tablo oluşturun, adını **"Metapsikoloji Başvurular"** koyun.
2. Menüden **Uzantılar → Apps Script**'i açın.
3. Editörde açılan kodun tamamını silin; `google-apps-script.js` dosyasının içeriğini yapıştırın.
4. En üstteki `BILDIRIM_EPOSTASI` satırına kendi e-posta adresinizi yazın.
5. Sağ üstten **Dağıt → Yeni dağıtım**:
   - Tür: **Web uygulaması**
   - Yürüten: **Ben (kendi hesabınız)**
   - Erişim: **Herkes** *(form ziyaretçileri giriş yapmadan gönderebilsin diye zorunlu)*
6. Çıkan izin ekranlarını onaylayın ve size verilen **Web uygulaması URL'sini** kopyalayın (`https://script.google.com/macros/s/.../exec`).
7. `index.html` içinde `BURAYA_APPS_SCRIPT_URL_YAPISTIRIN` yazan yeri bu URL ile değiştirin.
8. Test: Apps Script editöründe `testGonderimi` fonksiyonunu çalıştırın → Sheets'e satır düşmeli, e-posta gelmeli. Sonra sitedeki formu bir kez de gerçek olarak deneyin.

> Not: Form isteği tarayıcıdan `no-cors` modunda gönderilir; bu, Apps Script ile çalışmanın standart ve güvenilir yoludur.

## Adım 3 — Yer tutucuları gerçek bilgilerle değiştirin

`index.html` ve `tesekkurler.html` içinde arama-değiştirme yapmanız yeterli:

| Yer tutucu | Yerine gelecek | Nerede |
|---|---|---|
| `90XXXXXXXXXX` | Gerçek telefon (başında 90, örn. `905321234567`) | Tüm `wa.me` linkleri + `tel:+90...` |
| `BURAYA_APPS_SCRIPT_URL_YAPISTIRIN` | Adım 2'deki Web uygulaması URL'si | `index.html` script bölümü |
| `linkedin.com/company/metapsikoloji` | Kurumsal LinkedIn sayfanızın gerçek URL'si | Sosyal linkler + head'deki JSON-LD |
| `[50]` ve `[15]` | Gerçek seans ve ön görüşme süreleri | SSS bölümü |

**Görseller** (geldikçe `gorseller/` klasörüne yükleyin; HTML içindeki yorum satırları tam değişim noktasını gösterir):

- `logo.png` — şeffaf zeminli logo (header + footer)
- `uzman-fotograf.webp` — uzman fotoğrafı (4:5 oran, ~800×1000px önerilir)
- `sertifika-1.png` … `sertifika-4.png` — sertifika/kurum logoları
- `og-kapak.jpg` — sosyal medya paylaşım görseli (1200×630px)
- `online-seans.webp` — **hazır, bu pakette** (Seans Deneyimi bölümünde kullanılıyor; `gorseller/` klasörüne yükleyin)

> Hero şu an hafif SVG "sisli dağ" katmanları + buzlu cam panelle çalışıyor (0 KB görsel yükü). Fotoğrafik bir dağ manzarası tercih edersen lisanslı bir görseli (örn. Unsplash/Pexels, 1920px genişlik, <150 KB WebP) `gorseller/hero-dag.webp` adıyla ilet; tek geçişte entegre ederim.

## Adım 4 — Takip kodları

`index.html` ve `tesekkurler.html`'in `<head>` bölümünde işaretli üç blok var; kodları bu blokların içine olduğu gibi yapıştırın:

1. **GA4** (gtag.js) — her iki sayfaya da.
2. **Google Ads Global Site Tag** — her iki sayfaya da.
3. **Meta Pixel** — `index.html`'e; `noscript` parçası `<body>` açılışındaki işaretli alana.

### Google Ads uzmanı için dönüşüm ID tablosu

Tüm ID'ler statik HTML'e işlenmiştir; sayfa geçişinde/dinamik yapıda kaybolmaz.

| Element ID | Konum | Ölçtüğü aksiyon |
|---|---|---|
| `wa-kurumsal-conversion` | Header | Kurumsal WhatsApp başvurusu |
| `wa-hero-conversion` | Hero (ilk ekran) | Hero WhatsApp başvurusu *(HTML'de aynı ID iki kez kullanılamadığı için eklendi — panelde ayrı ölçülebilir ya da kurumsalla aynı dönüşüme bağlanabilir)* |
| `wa-uzman-conversion` | Uzman profili altı | Uzmana özel, güven odaklı WhatsApp başvurusu |
| `phone-direct-conversion` | Header | Doğrudan telefon araması |
| `ig-metapsikoloji-click` | Sosyal satırı | Kurumsal Instagram'a geçiş |
| `ig-eminsoydan-click` | Sosyal satırı | Uzman Instagram'ına geçiş |
| `li-metapsikoloji-click` | Sosyal satırı | Kurumsal LinkedIn'e geçiş |
| `li-eminsoydan-click` | Sosyal satırı | Uzman LinkedIn'ine geçiş |
| `form-submit-success` | Form butonu | Form gönderim tıklaması |

**Önemli ölçüm önerisi:** Asıl "form dönüşümünü" buton tıklamasına değil, **`/tesekkurler.html` sayfa görüntülemesine** bağlayın. Bu sayfa yalnızca form gerçekten başarılı olduğunda açılır; buton tıklaması ise doğrulama hatalarında da sayılabilir. `form-submit-success` ID'si yine de yerinde durur — iki yöntem de kullanılabilir. Ek olarak başarılı gönderimde `dataLayer`'a `form_submit_success` olayı da gönderilir (GTM kullanılırsa hazır).

### Kanal ölçümü (otomatik çalışır)

Sayfa, URL'deki `utm_source`, `utm_medium`, `utm_campaign`, `utm_term` ve `gclid` parametrelerini otomatik yakalar ve her form kaydıyla birlikte Sheets'e yazar. Böylece hangi başvurunun hangi reklamdan/kanaldan geldiğini tablodan doğrudan görürsünüz. Google Ads kampanya URL'lerine UTM eklemeniz yeterli.

## Adım 5 — Yayın öncesi kontrol listesi

- [ ] `https://www.metapsikoloji.tr` SSL ile açılıyor
- [ ] Telefon numaraları ve WhatsApp linkleri gerçek numarayla test edildi
- [ ] Form → Sheets'e satır düştü, e-posta geldi, `/tesekkurler` açıldı
- [ ] GA4 + Ads + Pixel kodları eklendi (Tag Assistant ile doğrulanabilir)
- [ ] Yasal sayfalar yüklendi (bir sonraki paket)
- [ ] Google Search Console'a alan adı eklendi

## Sırada ne var?

1. **Yasal sayfa taslakları** (Gizlilik, KVKK, Mesafeli Hizmet Sözleşmesi) — benden.
2. Görseller geldikçe **yerleştirme ve hız optimizasyonu** (WebP dönüşümü dahil) — benden.
3. Numara, e-posta ve kurumsal LinkedIn URL'si — sizden.
