---
title: "Sık Kullanılan Terminal Komutları"
date: 2026-09-01
tags: ["terminal", "linux", "komutlar"]
image: ""
---

Geliştiricilerin sık kullandığı temel terminal komutlarını derledim.

## Dosya İşlemleri

```bash
# Dosya listeleme
ls -la

# Dosya oluşturma
touch dosya.txt

# Dosya kopyalama
cp kaynak hedef

# Dosya taşıma/yeniden adlandırma
mv eski-yeni

# Dosya silme
rm dosya.txt

# Klasör oluşturma
mkdir klasor-adi
```

## Dizin İşlemleri

```bash
# Dizin değiştirme
cd /var/www/html

# Mevcut dizini gösterme
pwd

# Üst dizine çık
cd ..

# Kök dizine git
cd /
```

## Arama İşlemleri

```bash
# Dosya içinde arama
grep "aranan-terim" dosya.txt

# Dosya adında arama
find . -name "*.txt"

# İçerikte arama (daha gelişmiş)
rg "aranan-terim"
```

## Git Komutları

```bash
# Depoyu klonlama
git clone https://github.com/kullanici/depo.git

# Değişiklikleri ekleme
git add .

# Değişiklikleri kaydetme
git commit -açiklama "mesaj"

# Değişiklikleri gönderme
git push origin main

# Değişiklikleri çekme
git pull
```

## ipuçları

- `Ctrl + R`: Komut geçmişinde arama
- `Ctrl + C`: Çalışan komutu durdur
- `Ctrl + L`: Ekranı temizle
- `Tab`: Otomatik tamamlama

Bu komutları düzenli olarak kullanmaya başladığınızda işleriniz çok hızlanacak!
