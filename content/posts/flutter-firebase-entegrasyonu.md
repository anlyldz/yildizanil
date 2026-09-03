---
title: "Flutter-Firebase Entegrasyonu"
date: 2024-05-07
tags: ["Flutter", "Firebase", "Dart", "Mobil Geliştirme"]
image: "/images/post-flutter-firebase.jpeg"
summary: "Flutter ile yazılmış bir uygulamanın Firebase'e nasıl bağlanıp entegre edileceğini anlattım."
---

Merhaba, bu yazımda Flutter ile yazılmış bir uygulamanın Firebase'e nasıl bağlanıp entegre edileceğini anlatacağım.

Flutter, Google tarafından geliştirilen ve kullanıcı arayüzü oluşturmak için kullanılan açık kaynaklı bir mobil uygulama geliştirme framework'üdür. Firebase ise Google'ın sunucu tarafı platformudur ve uygulamalar için çeşitli hizmetler sunar.

Firebase Authentication, Flutter uygulamalarında kullanıcıların e-posta/şifre, Google veya Facebook gibi hesaplarla giriş yapmasını sağlar. Firebase Firestore veya Realtime Database ile veri tabanı işlemleri yapabilirsiniz.

### Adım 1: Firebase Projesi Oluşturma

[console.firebase.google.com](https://console.firebase.google.com/) adresi üzerinden siteye ulaştıktan sonra "Add project" butonuna basarak ilerleyin.

![Firebase Proje Oluşturma](/images/post-flutter-firebase.jpeg)

### Adım 2: Kullanılacak Platformu Seçme

Burada farklı platformlar üzerinden işlemler yapılabilir:

- IOS
- Android
- Web
- Unity
- Flutter

Flutter simgesine tıklayarak devam edin.

![Platform Seçimi](/images/post-flutter-firebase.jpeg)

Flutter'ı seçtikten sonra:

- Firebase CLI'nin yüklenmesi
- Flutter SDK'nın yüklenmesi
- Flutter projesi oluşturulması ya da olan projenin üstünden devam edilmesi

adımları uygulanmalıdır.

![Flutter Kurulum](/images/post-flutter-firebase.jpeg)

### Adım 3: Firebase CLI Kurulumu

Terminal üzerinden Firebase CLI'yı yükleyin:

```bash
npm install -g firebase-tools
```

Ardından Firebase'e giriş yapın:

```bash
firebase login
```

### Adım 4: FlutterFire CLI

Flutter projesini Firebase'e bağlamak için FlutterFire CLI kullanılır:

```bash
dart pub global activate flutterfire_cli
```

### Adım 5: Firebase Yapılandırması

```bash
flutterfire configure
```

Bu komut, Firebase projenizi otomatik olarak yapılandırır ve gerekli dosyaları oluşturur.

### Adım 6: Firebase Paketlerini Ekleme

`pubspec.yaml` dosyasına gerekli Firebase paketlerini ekleyin:

```yaml
dependencies:
  firebase_core: ^2.15.0
  firebase_auth: ^4.7.0
  cloud_firestore: ^4.8.0
```

### Adım 7: Firebase'i Başlatma

`main.dart` dosyasında Firebase'i başlatın:

```dart
import 'package:firebase_core/firebase_core.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}
```

### Sonuç

Flutter ve Firebase entegrasyonu, modern mobil uygulama geliştirme sürecini büyük ölçüde kolaylaştırır. Firebase'in sunduğu geniş hizmet yelpazesi ile uygulamanızı hızla geliştirebilirsiniz.
