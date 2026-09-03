---
title: "SIEM'de Güvenlik Olayları Nasıl Tespit Edilir?"
date: 2026-09-02
tags: ["SIEM", "Güvenlik", "SOC", "Log Analizi"]
image: "/images/post-siem-olay.jpg"
summary: "SIEM sistemlerinde binlerce log arasından gerçek güvenlik olaylarını nasıl tespit edebileceğimizi, korelasyon ve detection mantığını inceledim."
---

Önceki yazılarımda SIEM'in ne olduğunu, SOC yapılarındaki önemini ve SIEM sistemlerinin beslendiği temel veri kaynağı olan log yönetimini ele aldık.

Burada konu bakımından biraz daha detaya ineceğiz.
Topladığımız binlerce, hatta milyonlarca log arasından gerçekten bir güvenlik olayının gerçekleştiğini nasıl anlarız?

Büyük ya da küçük çoğu kurum ve kuruluşun altyapısında her saniye binlerce olay gerçekleşebilir.
Kullanıcılar sisteme giriş yapar, dosyalara erişir, uygulamalar çalışır, firewall bağlantılara izin verir veya engeller, endpoint'lerde işlemler gerçekleşir. Bunlar gibi daha birçok potansiyel senaryodan söz edebiliriz.

Asıl olay ise olağan aktivitelerin arasına karışan şüpheli davranışları ve saldırı belirtilerini tespit edebilmektir.

SIEM; farklı kaynaklardan gelen verileri toplar, anlamlandırır, ilişkilendirir ve belirli kurallar veya analiz yöntemleri doğrultusunda potansiyel güvenlik olaylarını ortaya çıkarır.

Bu yazıda, bir log kaydının nasıl güvenlik olayına dönüştüğünü, korelasyon ve detection mantığının bu süreçteki rolünü ve farklı güvenlik ürünlerinden gelen verilerin nasıl birlikte değerlendirilebildiğini inceleyeceğiz.

![SIEM](/images/post-siem-olay.jpg)

### Her Log Bir Güvenlik Olayı Değildir

SIEM'e gönderilen her logun bir saldırı anlamına gelmediğini tekrar belirtmek isterim.

Örneğin;

- Bir kullanıcının sisteme başarılı şekilde giriş yapması
- Bir firewall'ın bağlantıya izin vermesi
- Bir sunucuda bir servisin başlatılması
- Bir kullanıcının bir dosyaya erişmesi
- EDR'nin bir endpoint üzerinde şüpheli bir process tespit etmesi
- Trend Micro'nun zararlı bir dosyayı engellemesi

tek başına bakıldığında herhangi bir saldırı belirtisi olmayabilir.

Ancak aynı olayların belirli bir zaman aralığında, belirli bir kullanıcı, IP adresi veya cihaz ile birlikte gerçekleşmesi olayın anlamını değiştirebilir.

Örneğin:

**Tek bir başarısız login denemesi → Normal olabilir**

Ancak:

**2 dakika içerisinde aynı kullanıcıya karşı 100 başarısız login denemesi → Şüpheli olabilir**

Bu mantığı Splunk üzerinde basit bir sorguyla örnekleyebiliriz.

```spl
index=windows_index_isminiz EventCode=4625
| bin _time span=2m
| stats count as failed_attempts by _time, user, src_ip
| where failed_attempts >= 20
| sort - failed_attempts
```

Bu sorgu, Windows Security loglarında başarısız login olaylarını kullanıcı ve kaynak IP bazında gruplandırarak belirli bir eşik değerinin üzerindeki aktiviteleri göstermektedir.

Örneğin sonuç şu şekilde olabilir:

![Başarısız Login Sonuçları](/images/post-siem-olay.jpg)

Burada önemli olan nokta, 20 başarısız login görüldüğü için kesin olarak saldırı gerçekleştiğini söylememektir.

Bu sonuç yalnızca araştırılması gereken bir davranışın bulunduğunu gösterir.

Aynı aktivite yanlış yapılandırılmış bir servis, kullanıcının parolasını unutması veya bir güvenlik testi nedeniyle de gerçekleşebilir.

Dolayısıyla SIEM'in ürettiği sonuç, SOC analisti için çoğu zaman ilk araştırma noktasıdır.
**Bir sonuçtansa, sonucun bir başlangıcı denilebilir.**

### Korelasyon: SIEM'in En Önemli Yeteneklerinden Biri

SIEM'in güvenlik olaylarını tespit etmesinde en temel mekanizma **korelasyondur.**

Korelasyon, farklı kaynaklardan gelen olayların belirli kriterlere göre ilişkilendirilerek anlamlı bir güvenlik senaryosuna dönüştürülmesidir.

Mesela bir evin hırsızlık olayını düşün:

- Kapının gece saat 03.00'te açıldığı görülüyor.
- 2 dakika sonra evin salon ışığı yanıyor.
- Ardından evdeki değerli eşyanın bulunduğu dolap açılıyor.
- Son olarak evin kapısından biri çıkıyor.

Bunların her biri tek başına normal bir olay olabilir.

Ama bu olaylar birbiriyle ilişkilendirildiğinde, sistem şunu çıkarabilir:

"Gece saatlerinde kapı açıldı → evin içine girildi → değerli eşyalara erişildi → kişi evden çıktı. Muhtemelen hırsızlık gerçekleşti."

İşte bu olayları bir araya getirip tek ve anlamlı bir senaryoya dönüştürmeye **korelasyon** denir.

Örneğin bir saldırganın bir kullanıcı hesabına erişmeye çalıştığını düşünelim.

Öncelikle:

**Windows Event Log**

aynı kullanıcı için çok sayıda başarısız login denemesi gösterebilir.

Ardından:

**Windows Event Log**

aynı kullanıcı ve kaynak IP üzerinden başarılı bir login gerçekleştiğini gösterebilir.

Bu iki davranışı birlikte incelemek, yalnızca başarısız loginleri saymaktan çok daha anlamlı bir sonuç ortaya çıkarabilir.

Örneğin:

```spl
index=windows_index_isminiz (EventCode=4625 OR EventCode=4624)
| stats 
    count(eval(EventCode=4625)) as failed_logins
    count(eval(EventCode=4624)) as successful_logins
    by user, src_ip
| where failed_logins >= 10 AND successful_logins >= 1
```

![Korelasyon Sonucu](/images/post-siem-olay.jpg)

Burada SIEM yalnızca "çok fazla başarısız login var" demiyor.

Aynı zamanda:

**Başarısız loginler → Başarılı login**

ilişkisini ortaya çıkarıyor.

### Firewall Logları ile Ağ Davranışlarının Tespiti

SIEM'e gönderilen önemli veri kaynaklarından biri de firewall loglarıdır. (Çoğu zaman en önemlisi)

Firewall'lar Source(Kaynak) IP, Destination(Hedef) IP, Port, Protokol, Policy ve bağlantının izin verilip verilmediği gibi birçok bilgiyi sağlayabilir.

Örneğin kısa bir zaman aralığında aynı IP adresinden çok sayıda bağlantı denemesi isteği gerçekleşmesi birçok potansiyel davranışın araştırılmasını gerektirebilir. (örn: Port Scanning)

Basit bir SPL örneği:

```spl
index=firewall_index_isminiz action=blocked
| stats count by src_ip, dest_port
| where count >= 100
| sort - count
```

![Firewall Log Sonucu](/images/post-siem-olay.jpg)

Burada da dikkat etmemiz gereken şey tek bir firewall deny(red) logu güvenlik olayı olarak değerlendirilmez.

Ancak aynı kaynaktan çok sayıda bağlantı denemesi gelmesi, hedef portların çeşitlenmesi veya farklı sistemlere yönelik benzer aktivitelerin görülmesi olayın gidişatını değiştirebilir.

### Endpoint ve EDR Verileri ile Tehdit Tespiti

Öncelikle EDR temel olarak endpoint üzerinde gerçekleşen aktiviteleri izlemeye ve endpoint seviyesinde tehditleri tespit etmeye odaklanırken, SIEM farklı güvenlik ürünlerinden ve sistemlerden gelen verileri merkezi olarak bir araya getirerek daha geniş bir görünürlük ve korelasyon imkânı sağlar.

Modern SOC ortamlarında SIEM yalnızca firewall veya işletim sistemi loglarından beslenmez.

Örneğin Splunk'a EDR loglarının aktarıldığını varsayarsak, yüksek önem seviyesine sahip olayları basit bir sorguyla inceleyebiliriz:

```spl
index=edr_index_isminiz
| search action="blocked" OR severity="high"
| stats count by host, user, process_name, severity
| sort - count
```

### Şüpheli PowerShell Aktivitesinin Tespiti

Farklı bir örnek olarak **Endpoint loglarının** değerini göstermek için PowerShell üzerinden de basit bir örnek verebiliriz.

Windows ortamında PowerShell kullanılması tek başına zararlı değildir. Ancak PowerShell'in nasıl ve hangi süreç tarafından çalıştırıldığı önemli bir bağlam sağlayabilir.

Örneğin:

```spl
index=windows_index_isminiz EventCode=4688
| search process_name="*powershell.exe*"
| stats count by host, user, parent_process, command_line
| sort - count
```

![PowerShell Sonucu](/images/post-siem-olay.jpg)

Burada yalnızca PowerShell çalıştırılmış olması saldırı anlamına gelmez.

Fakat PowerShell'in bir Office uygulaması tarafından başlatılması, encoded command kullanılması veya aynı endpoint üzerinde EDR tarafından şüpheli bir davranış tespit edilmesi gibi ek göstergeler olayın risk seviyesini değiştirebilir.

### Sonuç

SIEM sistemlerinin asıl amacı milyonlarca logu tek bir yerde toplamak değildir.

Asıl amaç, bu büyük veri yığını içerisinden **güvenlik açısından anlamlı ve müdahale edilebilir davranışları ortaya çıkarmaktır.**

Bunu gerçekleştirmek için bu parametreler çok önemlidir;

- Doğru log kaynaklarının toplanması
- Logların doğru şekilde normalize edilmesi
- Detection ve korelasyon kurallarının oluşturulması
- Farklı güvenlik ürünlerinden gelen verilerin ilişkilendirilmesi
- False positive oranlarının azaltılması
- SOC analistlerinin olayları bağlam içerisinde değerlendirmesi

**Doğru Veri, Doğru Detection, Doğru Korelasyon ve Doğru Analiz Yaklaşımı** bulunur.

![Sonuç](/images/post-siem-olay.jpg)
