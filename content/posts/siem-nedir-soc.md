---
title: "SIEM Nedir? SOC İçin Neden Hayati Öneme Sahiptir?"
date: 2025-04-05
tags: ["SIEM", "SOC", "Güvenlik", "Siber Güvenlik"]
image: "https://cdn-images-1.medium.com/max/1024/1*1vMEklba13TvQCSKAerrIQ.png"
---

Teknolojinin gelişimiyle birlikte kurumsal ağlar büyüyor, buna paralel olarak güvenlik tehditleri de daha karmaşık hale geliyor. Artık yalnızca büyük şirketler değil, küçük ve orta ölçekli işletmeler de hedef alınan saldırıların etkisini ciddi şekilde hissediyor. Kurumlar yalnızca dış saldırganlara karşı değil, içeriden gelen tehditlere karşı da tetikte olmak zorunda.

Saldırganlar her geçen gün daha sofistike yöntemler kullanırken, sistemleri anlık olarak izleyip olaylara hızlı müdahale edebilmek her zamankinden daha önemli. İşte bu ihtiyaca yönelik geliştirilen sistemlerden biri de **SIEM** çözümleri. Bu yazıda, **SIEM'in ne olduğunu**, sağladığı temel işlevleri ve özellikle **SOC** yapıları içinde neden hayati öneme sahip olduğunu ele alacağım.

![SIEM ve SOC](https://cdn-images-1.medium.com/max/1024/1*1vMEklba13TvQCSKAerrIQ.png)

### SOC nedir?

SOC, "Security Operations Center" kelimesinin kısaltması ve Türkçe'de "Güvenlik Operasyon Merkezi" olarak adlandırılır.

**SOC (Security Operations Center)**, bir organizasyonun dijital altyapısını 7/24 izleyen, güvenlik tehditlerine karşı aktif bir savunma mekanizması sağlayan bir birimdir. Bugünün siber güvenlik tehditleri, genellikle sessizce ve karmaşık şekilde gelir. SOC, bu tehditleri **erken tespit etmek ve etkili şekilde müdahale etmek için sürekli bir gözetim sağlar.**

SOC ekipleri, çeşitli güvenlik araçları kullanarak ağlar, sistemler ve veritabanları üzerinde oluşan tüm olayları takip eder. Bu araçlar genellikle **SIEM**, **IDS/IPS** (Intrusion Detection/Prevention Systems) ve **firewall** çözümleriyle entegre çalışır.

![SOC Yapısı](https://cdn-images-1.medium.com/max/998/1*BwaFZaHbjNyGxChBEMjtEg.png)

### SIEM nedir?

SIEM, "Security Information and Event Management" (Güvenlik Bilgi ve Olay Yönetimi) ifadesinin kısaltmasıdır.

**SIEM (Security Information and Event Management)**, kurumların ağlarındaki güvenlik olaylarını ve sistem günlüklerini (log) merkezi bir noktada toplayan, analiz eden ve güvenlik ihlallerine karşı alarm üreten kapsamlı bir güvenlik teknolojisidir.

SIEM sistemleri; **firewall, antivirüs, sunucu, veritabanı, uygulama ve uç nokta cihazlar** ve daha birçok üründen gelen logları normalleştirerek merkezi bir yapıda birleştirir.

### SIEM'in Temel Yararları

- **Gerçek Zamanlı Tehdit Tespiti:** SIEM, sistemler üzerinden gelen tüm verileri gerçek zamanlı olarak izler ve tehditleri anında tespit eder.
- **Merkezi Olay İzleme:** Farklı ağ cihazlarından, uygulamalardan ve sistemlerden gelen log verilerini tek bir merkezi noktada toplar.
- **Proaktif Savunma:** Sadece mevcut tehditleri tespit etmekle kalmaz, aynı zamanda gelecekteki potansiyel riskleri öngörmeye çalışır.
- **Korelasyon ve Analiz:** Farklı güvenlik olaylarını birleştirerek karmaşık tehditler hakkında daha net bir resim oluşturur.
- **Otomatik Alarm ve Uyarılar:** Potansiyel tehditler, anında otomatik olarak alarm üretilir ve güvenlik ekiplerine bildirilir.
- **Log Yönetimi ve Arşivleme:** Güvenlik loglarını merkezi bir şekilde toplar ve uzun süre saklar.
- **Uyumluluk Yönetimi:** GDPR, PCI-DSS, ISO 27001 gibi yasal gerekliliklere uyum sağlamasına yardımcı olur.
- **İleri Düzey Raporlama:** Güvenlik olaylarının detaylı raporlarını sunar.
- **Veri Sızıntılarını Tespit Etme:** Ağ üzerindeki veri hareketlerini sürekli izler.
- **Anormal Davranış Analizi:** Kullanıcıların veya cihazların alışılmadık davranışlarını tespit eder.

### Sonuç olarak

SIEM ve SOC, siber güvenlik altyapılarının temel taşlarıdır. SIEM, tehditleri erken tespit ederken, SOC bu tehditlere hızlı ve etkin bir şekilde yanıt verir.

Güvenlik, sadece teknoloji değil, aynı zamanda doğru süreçler ve ekip çalışması gerektirir. Bu nedenle, SIEM ve SOC çözümlerinin başarısı, organizasyonun güvenlik kültürüyle doğrudan ilişkilidir.

### Sonraki Yazılarda Neler Var?

Bir sonraki yazılarımda:

- Korelasyon nedir? Korelasyon kuralları nasıl yazılır?
- Alarm eşikleri nasıl optimize edilir?
- SIEM ürünleri arasındaki farklar neler?
- Açık kaynak ve ticari SIEM'ler ne zaman tercih edilmeli?
- Global ve Yerli SIEM ürünleri nelerdir?

…gibi konulara daha teknik bir bakışla yaklaşacağım.
