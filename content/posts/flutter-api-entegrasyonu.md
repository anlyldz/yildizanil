---
title: "Flutter ile API Entegrasyonu: RESTful Servislerle Çalışmak"
date: 2024-05-20
tags: ["Flutter", "API", "REST", "Dart", "Mobil Geliştirme"]
image: "https://cdn-images-1.medium.com/max/925/1*ayRzCl9_ZCdVX5BZk10Q8w.jpeg"
summary: "RESTful API'ler, web servisleriyle iletişim kurmak için yaygın olarak kullanılan bir yöntemdir. Flutter ile API entegrasyonunu nasıl yapacağınızı bu yazıda anlattım."
---

RESTful API'ler, web servisleriyle iletişim kurmak için yaygın olarak kullanılan bir yöntemdir. Flutter, bu API'lerle etkileşim kurmak için güçlü ve esnek araçlar sunar. Bu blog yazımda, Flutter uygulamalarında RESTful API entegrasyonunu nasıl gerçekleştirebileceğimizi inceleyeceğiz.

Mobil uygulamaların çoğu, veri alışverişi yapmak ve çeşitli hizmetlerden yararlanmak için web servisleri ile etkileşimde bulunur. Bu etkileşim genellikle RESTful API'ler aracılığıyla gerçekleştirilir. RESTful API (Representational State Transfer), HTTP protokolünü kullanarak çeşitli uygulamalar arasında veri alışverişini mümkün kılan bir mimari tarzdır.

### 1. HTTP Paketi ile Çalışma

Flutter'da HTTP istekleri yapmak için popüler olarak kullanılan http paketi, RESTful API'lerle iletişim kurmak için en yaygın yöntemlerden biridir.

**HTTP paketini kurma**

```yaml
dependencies:
  http: ^0.13.3
```

**HTTP Paketini İçe Aktarma**

```dart
import 'package:http/http.dart' as http;
```

http paketi, Flutter uygulamanızda RESTful API'lerle etkileşim kurmanın güçlü ve esnek bir yolunu sunar. GET ve POST istekleri yaparak verileri çekebilir ve gönderebilirsiniz.

![HTTP Paketi](https://cdn-images-1.medium.com/max/1024/1*BvHEvnxeyYOExnpChgidUw.jpeg)

### 2. JSON Verilerini Çözme (Parsing)

API'lerden gelen veriler genellikle JSON formatında olur. Flutter'da bu verileri çözmek için `dart:convert` kütüphanesi kullanılır.

```dart
import 'dart:convert';

// JSON string'i Map'e dönüştürme
Map<String, dynamic> jsonData = json.decode(responseString);

// Map'i modele dönüştürme
User user = User.fromJson(jsonData);
```

### 3. GET İsteği Yapma

Basit bir GET isteği örneği:

```dart
Future<User> fetchUser() async {
  final response = await http.get(
    Uri.parse('https://api.example.com/users/1'),
  );

  if (response.statusCode == 200) {
    return User.fromJson(json.decode(response.body));
  } else {
    throw Exception('Kullanıcı yüklenemedi');
  }
}
```

### 4. POST İsteği Yapma

Yeni bir veri oluşturmak için POST isteği:

```dart
Future<User> createUser(String name, String email) async {
  final response = await http.post(
    Uri.parse('https://api.example.com/users'),
    headers: {'Content-Type': 'application/json'},
    body: json.encode({'name': name, 'email': email}),
  );

  if (response.statusCode == 201) {
    return User.fromJson(json.decode(response.body));
  } else {
    throw Exception('Kullanıcı oluşturulamadı');
  }
}
```

### 5. Hata Yönetimi

API entegrasyonunda hata yönetimi çok önemlidir:

```dart
try {
  final user = await fetchUser();
  // Kullanıcıyı göster
} catch (e) {
  // Hata mesajını göster
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(content: Text('Hata: $e')),
  );
}
```

### Sonuç

Flutter ile API entegrasyonu, modern mobil uygulamaların vazgeçilmez bir parçasıdır. Doğru yaklaşım ve araçlarla, güçlü ve verimli uygulamalar oluşturabilirsiniz.
