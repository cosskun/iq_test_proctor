# 🧠 IQ Master Proctor (Göz Takibi ve Güvenlikli Mobil IQ Testi)

Flutter ile geliştirilmiş, **On-Device Yapay Zeka (Göz & Dikkat Takibi)** ve **Kiosk / Sınav Güvenliği** mimarisine sahip yeni nesil IQ testi uygulaması.

---

## ✨ Temel Özellikler

1. **👁️ Canlı Göz ve Dikkat Takibi (Eye-Tracking):**
   * Kullanıcının gözleri ve kafa açısı ekrandan 2.5 saniyeden uzun süre ayrıldığında sınav otomatik olarak sonlandırılır.
   * Görüntüler sunucuya aktarılmaz, cihazın geçici belleğinde anlık olarak işlenir.
2. **🔒 Uygulama Kitleme ve Odak Koruması:**
   * Test sırasında başka bir uygulamaya geçiş yapıldığında veya arka plana atıldığında test anında başarısız sayılır.
3. **🎲 Dinamik Soru Havuzu:**
   * Kullanıcı başarısız olup tekrar sınava girdiğinde, daha önce gördüğü sorular veritabanı filtresiyle elenir ve tamamen yeni sorular getirilir.
4. **🚫 3 Başarısızlık Hakkı Limiti:**
   * Bir testten en fazla 3 kez başarısız olunabilir. 3. başarısızlığın ardından test kilitlenir ve 4. hak verilmez.
5. **📊 Zengin IQ Soru Havuzu:**
   * Raven Progressive Matrices (Görsel Matrisler)
   * Sayı Dizileri ve Matematiksel Mantık
   * Sözel Analoji ve Kıyas
   * Uzamsal / Boyutsal Zeka
   * Bilişsel Hız ve Bellek

---

## 📂 Dosya Yapısı

```
iq_test_proctor/
├── lib/
│   ├── data/
│   │   ├── iq_question_bank.dart   # Zengin IQ soru havuzu
│   │   └── default_tests.dart       # Test kategorileri ve tanımları
│   ├── models/
│   │   ├── question.dart            # Soru modeli ve kategoriler
│   │   ├── test_model.dart          # Test modeli
│   │   └── test_attempt.dart        # Sınav girişimleri ve sonuçları
│   ├── screens/
│   │   ├── home_screen.dart         # Ana sayfa ve test listesi
│   │   ├── consent_screen.dart      # KVKK ve Sınav Güvenlik Sözleşmesi
│   │   ├── test_screen.dart         # Sınav ekranı + Göz Takibi HUD
│   │   └── result_screen.dart       # Sonuç, IQ Skoru ve Kalan Hak Paneli
│   ├── services/
│   │   ├── proctor_service.dart     # Göz/Kafa ve Yaşam Döngüsü Denetleyicisi
│   │   ├── storage_service.dart     # Hak Sayacı ve Görülmemiş Soru Filtresi
│   │   └── firebase_service.dart    # Bulut Veritabanı Köprüsü
│   ├── widgets/
│   │   ├── proctor_hud.dart         # Canlı Kamera ve Dikkat Göstergesi
│   │   └── matrix_question_viewer.dart # Raven Matrisi Çizim Motoru
│   └── main.dart
├── baslat.bat                       # Tek tıkla web/mobil test başlatıcı
└── FIREBASE_KURULUM_REHBERI.md     # Firebase & Firestore kurulum rehberi
```
