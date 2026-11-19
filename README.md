# 🧭 İlRadar — Türkiye 81 İl Coğrafya ve Harita Oyunu

**İlRadar**, Türkiye'nin 81 ilini harita üzerinden keşfetmeye, tanımaya ve farklı oyun modlarıyla hız/dikkat becerilerini sınamaya odaklanan, **SwiftUI** ile geliştirilmiş dokunmatik bir iOS coğrafya oyunudur. Klavye girişine ihtiyaç duymadan doğrudan vektörel harita etkileşimi ve sezgisel dokunma kontrolleriyle çalışır.

---

## ✨ Öne Çıkan Özellikler

* **7 Farklı Oyun Modu:** Klasik keşiften zamana karşı yarışa, gizli haritadan coğrafi ve kültürel ipuçlarına kadar çeşitlenen modlar.
* **Klavyesiz, Harita Odaklı Etkileşim:** Tüm oyun akışı harita üzerinde dokunma, yakınlaştırma (pinch) ve kaydırma (pan) hareketleriyle yönetilir.
* **Önbellekli & GPU Hızlandırmalı Vektör Harita:** `tr-cities.geojson` verisinden üretilen 81 il sınır yolu önbelleğe alınır; yakınlaştırma ve kaydırma işlemleri GPU matris dönüşümleriyle akıcı şekilde işlenir.
* **Ustalık Pasaportu (`PassportStore`):** Uzmanlaşılan illeri (`X/81`), bölge bazlı ilerlemeleri, en iyi 60 saniye Blitz skorunu ve en uzun 3 Can serisini takip eder.
* **Dokunsal Geri Bildirim (Haptics):** Seçim ve etkileşimlerde sistem titreşimleri (`UISelectionFeedbackGenerator`, `UIImpactFeedbackGenerator`, `UINotificationFeedbackGenerator`) kullanılarak fiziksel bir kontrol hissi sunulur.
* **Koyu Tema & Buzlu Cam (Glassmorphism) Arayüz:** Haritayı merkeze alan koyu lacivert gradyan zemin, yarı saydam hedef kartı ve üst çubukta merkezi oyun modu menüsü yer alır.

---

## 🎮 Oyun Modları

| Mod | Açıklama | Odak |
| :--- | :--- | :--- |
| **Klasik** | 81 ilin tamamını haritada bulma modu. | Kapsamlı keşif ve seri (combo) |
| **Plaka Avı** | Verilen plaka kodundan ilgili ili haritada bulma modu. | Plaka bilgisi ve hız |
| **Gizli Harita (Blind Map)** | Bölge renkleri gizlenerek yalnızca il sınırları üzerinden tahmin yapma modu. | Sınır geometrisi ve konum hafızası |
| **Dedektif** | Coğrafi ipuçlarından yola çıkarak doğru ili yakalama modu. | Coğrafi akıl yürütme |
| **Kültürel Miras** | Tarihi ve kültürel ipuçlarından yola çıkarak ili bulma modu. | Kültür ve tarih bilgisi |
| **60sn Blitz** | 60 saniye içinde haritaya dokunarak en yüksek skora ulaşma modu. | Hız ve refleks |
| **Hayatta Kalma (Survival)** | Sınırlı can hakkıyla (3 hata hakkı) kesintisiz ilerleme modu. | Doğruluk ve risk yönetimi |

---

## 🗺️ Harita, Dokunma ve Arayüz Mimarisi

* **Vektörel Harita Çizimi (`TurkeyMapView` & `TurkeyGeoJSONLoader`):** İl poligonları ve merkez noktaları ekran boyutuna göre hesaplanıp önbelleklenir. Yakınlaştırma (`1.0x` – `6.0x`) ve kaydırma sırasında tüm noktalar yeniden hesaplanmak yerine doğrudan dönüşüm matrisi uygulanır; bulunan illerin plaka ve şehir etiketleri yakınlaştırma seviyesine göre dinamik ölçeklenir.
* **Çoklu Dokunma Yönetimi (`MapGestureOverlay`):** Tek/çift parmak kaydırma, kıstırarak yakınlaştırma ve tek dokunuş eşzamanlı olarak yönetilir; tıklanan ekran koordinatı ters dönüşümle temel harita koordinatına çevrilerek il eşleştirmesi yapılır.
* **Merkezi Mod Seçici (`modeSelectorMenu`):** Üst araç çubuğunda (`.principal`) yer alan tek kapsül menü ile modlar açıklamalarıyla birlikte listelenir; harita alanı geniş tutulurken yanlışlıkla mod değiştirme riski azaltılır.
* **Yüzen Hedef Kartı & İstatistik Barı:** Haritanın sol üst köşesinde aktif ipucu/hedef ve pas geçme (`skipCurrent`) butonu, alt kısımda ise bulunan il sayısı, skor ve moda göre süre/can/seri göstergeleri bulunur.
* **Tur Sonu ve Paylaşım:** Tur tamamlandığında skor ve bulunan il sayısı özetlenir; `ShareLink` ile metin olarak paylaşılabilir veya tek dokunuşla yeniden başlatılabilir.

---

## 🧱 Proje Yapısı

```text
İlRadar/
├── harita.xcodeproj/          # Xcode proje yapılandırması
└── harita/
    ├── haritaApp.swift         # Uygulama giriş noktası (@main)
    ├── ContentView.swift       # Ana ekran, mod menüsü, hedef kartı, istatistikler ve Pasaport ekranı
    ├── GameEngine.swift        # Oyun durumları, mod kuralları, skor/can/süre yönetimi
    ├── Models.swift            # İl, bölge, oyun modu ve pasaport veri modelleri (`PassportStore`)
    ├── TurkeyMapView.swift     # Önbellekli Canvas harita çizimi ve çoklu dokunma katmanı
    ├── TurkeyGeoJSONLoader.swift # `tr-cities.geojson` ayrıştırıcı ve geometri yardımcıları
    ├── tr-cities.geojson       # Türkiye 81 il sınır verisi
    └── Assets.xcassets/        # `AppIcon` (1024x1024) ve renk varlıkları
