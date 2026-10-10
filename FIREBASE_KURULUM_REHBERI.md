# 🧠 IQ Master Proctor - Veritabanı ve Kurulum Rehberi

Bu rehber, **IQ Master Proctor** uygulamasının mimarisini, veritabanı (Firebase / Supabase) kurulum adımlarını ve mobil izin yapılandırmasını içerir.

---

## 🏗️ 1. Veritabanı Mimarisi ve Tablo / Koleksiyon Yapısı

Veritabanında **4 ana koleksiyon (tablo)** yer almaktadır:

### A. `tests` (Test Tanımları)
```json
{
  "testId": "test_standard_iq",
  "title": "Genel Bilişsel IQ Testi",
  "subtitle": "Standart Raven & Mantıksal Yetenek Ölçeği",
  "durationMinutes": 20,
  "totalQuestionsPerSession": 15,
  "maxFailureAttempts": 3
}
```

### B. `questions` (Soru Havuzu)
```json
{
  "id": "num_01",
  "testId": "test_standard_iq",
  "category": "numberSequences",
  "type": "multipleChoiceText",
  "prompt": "3, 7, 15, 31, 63, ?",
  "options": ["127", "126", "125", "129"],
  "correctOptionIndex": 0,
  "explanation": "Her terim bir öncekinin 2 katının 1 fazlasıdır (x * 2 + 1).",
  "difficultyLevel": 2
}
```

### C. `user_seen_questions` (Görülen Sorular - Tekrarı Engelleme)
```json
{
  "userId": "user_abc123",
  "testId": "test_standard_iq",
  "seenQuestionIds": ["num_01", "logic_02", "verb_03", "matrix_01"]
}
```

### D. `test_attempts` (Sınav Girişimleri & 3 Başarısızlık Limiti)
```json
{
  "id": "att_1710000000",
  "userId": "user_abc123",
  "testId": "test_standard_iq",
  "attemptNumber": 1,
  "status": "failedGazeLost",
  "failReasonDetail": "Gözler 2.5 saniyeden uzun süre ekrandan ayrıldı.",
  "startedAt": "2026-10-08T10:00:00Z",
  "finishedAt": "2026-10-08T10:04:12Z",
  "correctAnswers": 4,
  "totalQuestions": 15
}
```

---

## 🔥 2. Firebase Kurulumu (Adım Adım)

Uygulamayı Firebase Bulut Veritabanına bağlamak için:

1. [Firebase Console](https://console.firebase.google.com/) adresine gidin ve **Yeni Proje Oluştur** butonuna tıklayın.
2. **Firestore Database** sekmesinden yeni bir veritabanı oluşturun.
3. Terminalde FlutterFire CLI aracını yükleyin:
   ```bash
   dart pub global activate flutterfire_cli
   ```
4. Proje dizininde (`C:\Users\Developer6\.gemini\Project\iq_test_proctor`) komutu çalıştırarak Firebase'i bağlayın:
   ```bash
   flutterfire configure
   ```
5. `pubspec.yaml` dosyasına Firebase kütüphanelerini ekleyin:
   ```yaml
   dependencies:
     firebase_core: ^3.6.0
     cloud_firestore: ^5.4.4
     firebase_auth: ^5.3.1
   ```

---

## 🛡️ 3. Firestore Güvenlik Kuralları (Security Rules)

Kullanıcıların kendi haklarını veya soru cevaplarını manipüle etmesini önlemek için aşağıdaki kuralları Firebase Console'a yapıştırın:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // Testler ve Soru Havuzu: Sadece Okunabilir (Cevaplar gizlenebilir)
    match /tests/{testId} {
      allow read: if true;
      allow write: if false;
    }
    
    match /questions/{questionId} {
      allow read: if request.auth != null;
      allow write: if false;
    }

    // Kullanıcı Denemeleri: Kullanıcı sadece kendi denemelerini okuyabilir
    match /test_attempts/{attemptId} {
      allow read, create: if request.auth != null && request.auth.uid == request.resource.data.userId;
      allow update, delete: if false; // Geçmiş sınavlar değiştirilemez!
    }
    
    // Görülen Sorular Listesi
    match /user_seen_questions/{docId} {
      allow read, write: if request.auth != null;
    }
  }
}
```

---

## 📱 4. Mobil Kamera & Kiosk İzinleri

### Android (`android/app/src/main/AndroidManifest.xml`)
```xml
<uses-permission android:name="android.permission.CAMERA" />
<uses-feature android:name="android.hardware.camera" android:required="true" />
<uses-feature android:name="android.hardware.camera.front" android:required="true" />
```

### iOS (`ios/Runner/Info.plist`)
```xml
<key>NSCameraUsageDescription</key>
<string>Sınav esnasında dikkat ve göz takibini cihaz üzerinde gerçekleştirmek için ön kamera izni gereklidir.</string>
```

---

## 🚀 5. Uygulamayı Başlatma

Klasör içerisindeki **`baslat.bat`** dosyasına çift tıklayarak uygulamayı anında tarayıcıda çalıştırabilir ve göz takibi / sınav akışını test edebilirsiniz.
