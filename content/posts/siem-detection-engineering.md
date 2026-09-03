---
title: "SIEM'de Detection Engineering: Log Verisinden Anlamlı Alarmlara Giden Süreç"
date: 2026-03-31
tags: ["SIEM", "Detection", "SOC", "Güvenlik", "Engineering"]
image: "/images/post-siem-detection.png"
summary: "Log verisinden anlamlı güvenlik alarmları nasıl üretilir? Detection engineering süreçlerini SIEM, EDR ve SOAR üzerinden inceledim."
---

Günümüz siber güvenlik operasyonlarında, tek başına log verisi, anlamlı bir güvenlik aksiyonu üretmek için yeterli değildir. Kurumlar her gün milyonlarca log üretirken, bu verilerin içerisinden gerçek tehditleri ayıklamak kritik bir mühendislik problemine dönüşmüştür.

Peki bu kadar veri arasında hangisi gerçekten tehlikeli?

SIEM (Security Information and Event Management) sistemleri, bu noktada devreye girerek farklı kaynaklardan toplanan log verilerini normalize eder (parsing, enrichment, timestamp correction, deduplication), korele eder ve anlamlı güvenlik alarmlarına dönüştürür. Ancak bu dönüşüm süreci, yalnızca veri toplamakla sınırlı değildir; doğru kural yazımı, davranış analizi ve bağlamsal değerlendirme gerektirir.

Detection engineering, yalnızca loglardan alarm üretmek değildir. Asıl amaç, saldırgan davranışlarını anlayarak bu davranışları yakalayabilecek mantığı tasarlamaktır. Bu süreçte en büyük zorluk, milyonlarca normal olay arasından anlamlı sinyalleri ayıklayabilmektir. Başarılı bir detection, yalnızca bir eventi değil, bir hikâyeyi yakalar.

Gerçek zorluk, milyonlarca "normal" davranış arasından anormal olanı yakalayabilmektir. Bu yazıda, bir log kaydının nasıl anlamlı bir güvenlik alarmına dönüştüğünü; SIEM, EDR ve SOAR ekosistemi üzerinden gerçek hayata yakın örneklerle inceleyeceğiz.

![Detection Engineering](/images/post-siem-detection.png)

### 1. Log Toplama ve Normalizasyon

Bir güvenlik olayının tespiti aslında oldukça basit bir noktadan başlar: log. Sistemlerde gerçekleşen her aksiyon — bir kullanıcının giriş yapması, bir servisin başlatılması, bir isteğin sunucuya ulaşması — bir log kaydı olarak tutulur.

SIEM'in ilk görevi, farklı kaynaklardan gelen logları toplamaktır:

- Firewall logları
- Windows Event Log
- Web server logları
- EDR telemetry
- Uygulama logları

Ancak bu verilerin doğrudan analiz edilmesi mümkün değildir. Çünkü her kaynak farklı formatta veri üretir. Bu yüzden SIEM sistemleri logları normalize eder, yani ortak bir dile çevirir.

Örnek bir Splunk sorgusu:

```spl
index=wineventlog EventCode=4625 
| stats count by user, src_ip
```

Bu sorgu bize başarısız login denemelerini verir.

Ama dikkat:
Bu hâlâ sadece **log**, henüz bir "alarm" değil.

### 2. SIEM Ne Zaman Şüphelenir?

SIEM'in asıl değeri, tekil logları analiz etmekten ziyade bu loglar arasındaki ilişkileri anlamlandırabilmesidir. Çünkü gerçek hayatta saldırılar genellikle tek bir olayla kendini belli etmez.

Örneğin bir kullanıcının bir kez hatalı şifre girmesi tamamen normaldir. Ancak aynı IP adresinden kısa bir süre içerisinde onlarca farklı kullanıcı hesabına giriş denemesi yapılması artık sıradan bir davranış değildir.

Splunk ile:

```spl
index=wineventlog EventCode=4625
| bucket _time span=5m
| stats dc(user) as user_count count by src_ip, _time
| where user_count > 10
```

Bu noktada SIEM şunu söyler:

> *"Bu normal değil."*

Artık elimizde bir **brute force sinyali**, yani bir **alarm adayı var.**

### 3. False Positive vs True Positive: En Kritik Denge

Bir SIEM sisteminin başarısı, ürettiği alarm sayısından çok, bu alarmların doğruluğu ile ölçülür.

False positive, yani yanlış alarm, genellikle sistemin normal bir davranışı tehdit olarak algılaması sonucu oluşur.

**False Positive Nasıl Azaltılır?**

- Whitelisting (güvenilir IP'ler)
- Threshold tuning (eşik ayarı)
- Asset & user enrichment (CMDB, rol bilgisi)
- Multi-source correlation (tek log yerine çok kaynak)

### 4. EDR: İşin Derinliği

SIEM sistemleri ağ ve sistem logları üzerinden analiz yaparken, bazı saldırı teknikleri bu katmanın ötesine geçer. Özellikle saldırganlar sistem içine sızdıktan sonra, faaliyetlerini gizlemek için yerleşik araçları kullanabilirler.

Bu noktada EDR (Endpoint Detection and Response) çözümleri devreye girer. EDR, doğrudan endpoint üzerinde çalışan süreçleri, komutları ve davranışları izler.

SIEM ve EDR birlikte kullanıldığında, güvenlik ekipleri hem ağ seviyesinde hem de endpoint seviyesinde tam görünürlük elde eder.

### 5. Gerçek Hayat Senaryosu (End-to-End)

Gerçek bir saldırı senaryosu genellikle tek bir olaydan değil, bir olay zincirinden oluşur:

1. Saldırgan brute force yapar → SIEM alarm üretir
2. Başarılı login gerçekleşir → SIEM ciddiyeti artırır
3. EDR şüpheli komut tespit eder → PowerShell encoded command
4. Lateral movement başlar → Farklı sunuculara erişim
5. SOAR otomatik müdahale eder → Hesap devre dışı, IP engeli

![End-to-End Süreç](/images/post-siem-detection.png)

### Sonuç

SIEM sistemleri çoğu zaman "log toplayan araçlar" olarak düşünülse de, gerçekte çok daha fazlasını ifade eder.

> **SIEM bir alarm üretim sistemi değil, bir "anlamlandırma" sistemidir.**

Ve bu anlamlandırma ne kadar doğru yapılırsa, güvenlik operasyonları o kadar proaktif, hızlı ve etkili hale gelir.
