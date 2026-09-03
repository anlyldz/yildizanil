---
title: "Hugo ile Blog Açma Rehberi"
date: 2026-09-02
tags: ["hugo", "web", "blog", "rehber"]
image: ""
---

Hugo, hızlı ve modern bir statik site üreticisidir. Bu yazıda Hugo ile nasıl blog açılacağını anlatacağım.

## Hugo Nedir?

Hugo, Go dilinde yazılmış açık kaynaklı bir statik site üreticisidir. Diğer blog platformlarına göre birçok avantajı vardır:

- **Hız**: Milisaniyeler içinde site oluşturur
- **Güvenlik**: Sunucu tarafı kod çalıştırmaz
- **Basitlik**: Karmaşık kurulum gerektirmez
- **Esnek**: Çok çeşitli tema seçenekleri

## Kurulum

```bash
# macOS için
brew install hugo

# Yeni site oluşturma
hugo new site blogum
cd blogum

# Tema ekleme
git init
git submodule add https://github.com/Livour/hugo-mana-theme.git themes/mana
```

## Yazı Yazma

Yeni bir yazı oluşturmak için:

```bash
hugo new content posts/yazi-basligi.md
```

Bu komut `content/posts/` altında yeni bir Markdown dosyası oluşturur.

## Önizleme

Yerel sunucuda siteyi önizlemek için:

```bash
hugo server -D
```

Tarayıcınızda `http://localhost:1313` adresini açmanız yeterli.

## Dağıtma

Siteyi üretmek için:

```bash
hugo --minify
```

Oluşturulan dosyalar `public/` klasöründe bulunur. Bu dosyaları GitHub Pages, Netlify veya Vercel'e yükleyebilirsiniz.

## Sonuç

Hugo ile blog açmak hem kolay hem de eğlenceli. Bir sonraki yazımda daha detaylı konulara değineceğim.
