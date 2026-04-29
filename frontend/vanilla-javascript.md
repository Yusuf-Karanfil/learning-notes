# Vanilla JavaScript

**Amaç:** JS Frameworklerine zemin hazırlamak, kod okuyabilmek.

---
## 1. Temellerr

### Değişkenlerr
```js
var isim = "Yusuf";   // ❌ Block scope yok
let yas = 25;          // ✅
const sehir = "Ankara"; // ✅ Sabit değer
```

### Veri Tiplerii
| Değer | Tip |
|-------|-----|
| "Yusuf" | string |
| 25 | sayı |
| true | boolean |
| null | boş |

### Karşılaştırma
```js
5 == "5"  // true  → sadece değere bakar
5 === "5" // false → değer + tipe bakar
```

### Ternary (Tek Satırda if/else)
```js
const mesaj = yas >= 18 ? "Yetişkin" : "Genç";
```

---

## 2. Fonksiyonlar

Bir kod bloğunu bir kez yazıp defalarca çalıştırmaya yarar..

```js
// Declaration → Eski yol
function selamla(isim) {
  return "Merhaba, " + isim;
}

// Expression → Fonksiyonu değişken gibi tanımlar
const selamla = function(isim) {
  return "Merhaba, " + isim;
}

// Arrow Function → ES6, tek satırda fonksiyon
const selamla = (isim) => "Merhaba, " + isim;

console.log(selamla("Yusuf"));
```

---

## 3. Diziler & Objeler

### Objeler
Gerçek dünyada bir varlığı temsil eder. Kullanıcı, ürün, obje vs.
```js
const kullanici = {
  isim: "Yusuf",
  yas: 25,
  sehir: "Ankara",
  aktif: true
};
```

### Diziler
Sıralı veri listesi. Objelerin koleksiyonu.
```js
const isimler = ["Yusuf", "Ahmet", "Mehmet"];
```

### Dizi Metodları
```js
// forEach → her elemana bir şey yap
isimler.forEach(isim => console.log(isim));

// map → her elemanı dönüştür, yeni dizi döndür
const sayilar = [1, 2, 3, 4];
const kareler = sayilar.map(sayi => sayi * sayi);
// → [1, 4, 9, 16]
// ⭐ React'ta bir listeyi ekrana basmak için map kullanılır.

// filter → koşulu sağlayanları al, yeni dizi döndür
const yaslar = [15, 22, 17, 30, 13];
const yetiskinler = yaslar.filter(yas => yas >= 18);
// → [22, 30]

// find → koşulu sağlayan ilk değeri döndür
// reduce → diziyi tek bir değere indir
```

---

## 4. DOM Manipülasyonu

DOM (Document Object Model): Tarayıcı HTML'i okuyunca onu bir ağaç yapısına çevirir. Her etiket bir node olur. JS bu ağaca erişip değiştirebilir.

⭐ JS, bir HTML yapısına `document` objesi üzerinden ulaşır.

### Element Seçme
```js
const baslik = document.getElementById("baslik");
const altbaslik = document.querySelector(".altbaslik");
const tumParagraf = document.querySelectorAll("p"); // Tüm p'leri kapsar
```

### İçerik & Stil Değiştirme
```js
baslik.textContent = "Yeni Başlık";
baslik.innerHTML = "<span>Yeni</span>";
baslik.style.color = "red";
baslik.style.fontSize = "24px";

baslik.classList.add("aktif");       // Bu class'ı ekle
baslik.classList.remove("gizli");    // Bu class'ı kaldır
baslik.classList.toggle("secili");   // Varsa kaldır, yoksa ekle
baslik.classList.contains("aktif"); // true/false döner
```

### Event Listener
Kullanıcının girdisini bekler.
```js
const buton = document.querySelector("#buton");
buton.addEventListener("click", function() { console.log("Tıklandı"); });
```

| Event | Ne zaman tetiklenir |
|-------|---------------------|
| `click` | tıklama |
| `submit` | form gönderme |
| `input` | input değişimi (tuşa basıldığında) |
| `change` | input odaktan çıkınca değişim |
| `mouseover` | fare üzerine gelince, hover |
| `keydown` | tuşa basınca |
| `DOMContentLoaded` | HTML tamamen yüklenince |

---

## 5. Asenkron JS

JS tek thread'li çalışır. Bir işlem bitene kadar diğerini bekler. Ama veri çekmek, dosya okumak gibi işlemler zaman alır. Bu süre boyunca sayfa donarsa kullanıcı deneyimi mahvolur.

Çözüm: "Bu işlemi başlat, bitmesini bekleme, bitince şunu yap." → Asenkron programlama.

### async/await
```js
async function kullaniciGetir() {
  const sonuc = await fetch("https://...");
  const veri = await sonuc.json();
  console.log(veri);
}
kullaniciGetir();
```

### fetch — API'dan Veri Çekmek
Tarayıcının yerleşik HTTP istek fonksiyonu.
```js
// POST isteği → veri göndermek
JSON.stringify() // JS objesini JSON string'e çevirir
yanit.json()     // JSON string'i JS objesine çevirir
```

---

## 6. Closure & Scope

### Scope
- Fonksiyon içindeyken dışarıyı görebilir.
- Fonksiyon dışı, fonksiyon içini göremez.

### Closure
Değişkenleri, fonksiyon bitse bile hatırlar.

---

## 7. ES6+ Özellikleri

### Template Literals
String birleştirmenin modern yolu. Backtick (`` ` ``) kullanır.
```js
// Eski yol
const mesaj = "Merhaba " + isim + " Yaş: " + yas;

// Template Literal
const mesaj = `Merhaba ${isim} Yaş: ${yas}`;
```

### Optional Chaining (`?.`)
Derin obje erişiminde hata almamak için kullanılır.
```js
const posta = kullanici?.adres?.posta;
// ?.  solundaki değer null veya undefined ise hata fırlatmaz, boş bırakır.
```

### Nullish Coalescing (`??`)
null veya undefined ise `??` sağ tarafını kullan.
```js
const email = kullanici?.iletisim?.email ?? "Email yok";
```

---

## 8. Hata Yönetimi

Kod her zaman planlandığı gibi çalışmaz. API yanıt vermez, kullanıcı yanlış veri girer, ağ kesilir vs. Sistem çöker.

### try / catch / finally
```js
try {
  // hata çıkabilecek kod buraya
} catch (hata) {
  // hata oluşursa burası çalışır
} finally {
  // hata olsa da olmasa da çalışır
}
```
