---
title: "Siber Güvenliğin Omurgası: SIEM ve Log Yönetiminin Temel Kavramları ve Kritik Rolü"
date: 2026-02-02
tags: ["SIEM", "Log Yönetimi", "Güvenlik", "SOC"]
image: "https://cdn-images-1.medium.com/max/1024/1*pqqH-HiSd3Wm1BV9IDo2Vw.png"
summary: "SIEM ve log yönetimi arasındaki ilişkiyi, temel kavramları ve SOC yapıları için neden kritik olduğunu ele aldım."
---

Siber güvenlik ekosisteminde **SIEM** kavramı neredeyse her ortamda karşımıza çıkar. Ancak bu kavramın gerçekten ne ifade ettiğini kavrayabilmek için, doğru soruyu sorabilmemiz gerekir:
**Bir SIEM sistemi tam olarak neyi izler ve bu verileri hangi kaynaklardan toplar?**

Bu sorunun cevabı oldukça nettir: **loglar**.

Log yönetimi, SIEM sistemlerinin temelini oluşturur. Log olmadan SIEM, güvenlik operasyonları için anlamlı bir çıktı üretemez. Bu yazıda, SIEM ile log yönetimi arasındaki ilişkiyi, temel kavramları ve neden SOC yapıları için kritik olduğunu ele alacağız.

![SIEM ve Log Yönetimi](https://cdn-images-1.medium.com/max/1024/1*pqqH-HiSd3Wm1BV9IDo2Vw.png)

### Log Nedir?

Log; bir sistem, uygulama veya cihaz üzerinde gerçekleşen olayların kayıt altına alınmış halidir, kaba tabirle dijital ayak izidir.
Bu olaylar; bir kullanıcı giriş denemesi, bir bağlantı isteği, bir hata mesajı ya da bir yetkilendirme değişikliği olabilir.

Loglar; sistemlerde gerçekleşen her olayın, her etkileşimin ve her anomali ihtimalinin dijital izleridir. SIEM sistemleri bu izleri farklı kaynaklardan toplayarak anlamlı hale getirir. Ancak burada önemli bir gerçek vardır: **log yönetimi olmadan SIEM, yalnızca boş bir analiz kabuğundan ibaret kalır.**

Basit bir örnekle:

- Bir kullanıcı yanlış şifre girer → **log oluşur**
- Firewall bir bağlantıyı engeller → **log oluşur**
- Bir servis beklenmedik şekilde durur → **log oluşur**

### Log Yönetimi Nedir?

Modern BT altyapılarında her sistem, her uygulama ve her güvenlik ürünü sürekli olarak **log** üretir. Bu loglar; kullanıcı aktivitelerinden sistem hatalarına, ağ trafiğinden güvenlik ihlali girişimlerine kadar pek çok kritik bilgiyi içerir.

**Log yönetimi**, farklı kaynaklardan üretilen bu logların **toplanması, normalize edilmesi, güvenli şekilde saklanması ve analiz edilmesi** sürecidir.

### Log Yönetimi Sürecinin Temel Adımları

Etkili bir log yönetimi genellikle aşağıdaki aşamalardan oluşur:

- **Logların farklı sistemlerden toplanması:** Sunucular, ağ cihazları, güvenlik duvarları, uygulamalar ve uç noktalar gibi pek çok kaynaktan veri alınır.
- **Farklı formatların normalize edilmesi:** Her üretici ve sistem logları farklı formatta üretir. Bu logların ortak bir yapıya dönüştürülmesi kritiktir.
- **Logların güvenli ve ölçeklenebilir şekilde saklanması:** Logların hem bütünlüğü korunmalı hem de yasal gereksinimlere uygun sürelerde saklanabilmelidir.
- **Anlamlı olayların tespit edilmesi:** Büyük veri yığını içinden gerçekten önemli olan olayların ayıklanması.

![Log Yönetimi Adımları](https://cdn-images-1.medium.com/max/722/1*UhY5V9OYyKyPuLHGcEM1dw.png)

### SIEM ve Log Yönetimi Arasındaki İlişki

**SIEM (Security Information and Event Management)**, temelinde merkezi bir log toplama ve analiz platformu olarak çalışır.

SIEM sistemleri;

- Logları **merkezi bir noktada** toplar
- Farklı kaynaklardan gelen veriler arasında **korelasyon** kurar
- Olası **güvenlik olaylarını ve tehditleri** tespit eder
- **Alarm ve uyarılar** üreterek SOC ekiplerine görünürlük sağlar

Kısacası; **log yönetimi, SIEM'in ham maddesidir.**

### SIEM'de En Sık Karşılaşılan Log Türleri

**Ağ Cihazları:**
- Firewall
- IDS / IPS
- Switch ve Router'lar

**İşletim Sistemleri:**
- Windows Event Log
- Linux / Unix syslog

**Uygulamalar ve Sunucular:**
- Web server logları (Apache, Nginx, IIS)
- Uygulama logları
- Veritabanı erişim ve sorgu logları

**Güvenlik Ürünleri:**
- Antivirüs / EDR / XDR çözümleri
- DLP (Data Loss Prevention)
- E-posta güvenlik sistemleri

### Sonuç

SIEM sistemleri, siber güvenlik operasyonlarının merkezinde yer alsa da gerçek gücünü **beslendiği logların kalitesi ve doğruluğundan** alır.

Sağlam bir log yönetimi temeli üzerine kurulan SIEM, SOC ekipleri için yalnızca bir izleme aracı değil; **erken uyarı, hızlı müdahale ve güçlü bir siber savunma mekanizması** haline gelir.
