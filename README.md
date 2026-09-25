# LED Ekran Simülasyonu (Three.js)

Bir işyeri cephesine monte edilmiş LED ekranı gerçek zamanlı olarak simüle eden, tek dosyalık bir Three.js uygulaması.

## Çalıştırma

`index.html` dosyasını tarayıcıda açmanız yeterli (Three.js CDN'den yüklenir, internet bağlantısı gerekir).
İsterseniz yerel bir sunucuyla da açabilirsiniz:

```bash
python3 -m http.server 8000
# http://localhost:8000
```

## Parametreler (sağ üstteki panel)

**Ekran boyutu ve konumu**
- Genişlik / Yükseklik (metre)
- Cephedeki yatay konum ve yerden yükseklik

**Çözünürlük**
- Hazır çözünürlükler (64×32 … 3840×2160)
- Yatay / dikey piksel sayısı, piksel aralığı (pitch, mm)
- *Kare piksel (oranı koru)*: açıkken piksel aralığı sabit tutulur; ekran büyütülünce LED sayısı artar (kabin eklemek gibi). Hazır çözünürlük seçildiğinde yükseklik, kare piksel olacak şekilde ayarlanır.

**Görsel / içerik**
- Hazır içerikler: test deseni, kampanya, kayan yazı, renkli animasyon, saat
- Bilgisayardan görsel veya video yükleme (dosyayı sayfaya sürükleyip bırakmak da mümkün)
- URL'den görsel/video yükleme (sunucunun CORS izni vermesi gerekir)
- Yerleşim: sığdır / doldur (kırp) / esnet
- Kayan yazı metni

**LED görünümü**
- Parlaklık, LED doluluk oranı, LED şekli (yuvarlak DIP / kare SMD)
- LED piksel yapısını göster/gizle, görüş açısına bağlı parlaklık kaybı

**Sahne ve kamera**
- Gündüz / gece modu, ışıma (bloom)
- Hazır kamera görünümleri: cadde, karşı kaldırım, ekrana yakın, LED detayı

Sol üstteki bilgi panelinde piksel aralığı, toplam piksel, piksel yoğunluğu, önerilen izleme mesafeleri,
500×500 mm kabin sayısı ve kameranın ekrana uzaklığı gösterilir.

## Nasıl çalışıyor?

LED paneli özel bir shader ile çizilir: panel, çözünürlük kadar hücreye bölünür; her hücre görselin kendisine
denk gelen bölgesinin ortalama rengini (mipmap) gösterir ve yuvarlak/kare bir LED maskesiyle boyanır.
Uzaktan bakıldığında LED'ler ekran pikselinden küçük kaldığında maske, moiré oluşmaması için ortalama
kaplama oranına yumuşakça geçer.
