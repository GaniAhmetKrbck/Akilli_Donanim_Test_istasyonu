# 🚀 Akıllı Donanım Test İstasyonu (Smart Hardware Test Station)

**Akıllı Donanım Test İstasyonu**; gömülü sistemler, roket aviyonikleri (TEKNOFEST vb.), motor testleri ve endüstriyel otomasyon projeleri için geliştirilmiş, yüksek performanslı, evrensel bir **Veri Toplama (DAQ) ve Canlı Telemetri Arayüzüdür**. 

Herhangi bir mikrodenetleyiciden (STM32, ESP32, Arduino vb.) UART (Seri Port) üzerinden gelen verileri anlık olarak yüksek akıcılıkla görselleştirir, istatistiklerini tutar, CSV olarak kaydeder ve analiz etmenizi sağlar.

---

## ✨ Öne Çıkan Özellikler

* **Gerçek Zamanlı Çoklu Kanal Çizimi:** Gelen verileri eşzamanlı olarak farklı renklerde, PyQtGraph altyapısıyla donanıma yük bindirmeden akıcı bir şekilde grafikleştirir.
* **Akıllı Filtreleme (Ön Ek & Ayraç Desteği):** Mikrodenetleyiciden gelen debug/hata yazıları ile gerçek telemetri verilerini birbirinden ayırmak için **Prefix (Ön Ek)** ve **Delimiter (Ayraç)** seçimi sunar. Sadece istenen veri satırları işlenir.
* **Canlı İstatistik Paneli:** Her kanal için anlık, minimum, maksimum ve ortalama değerleri saniyesi saniyesine günceller.
* **Dinamik Eşik ve Alarm Sistemi:** Belirlenen değerler aşıldığında hem grafik üzerinde kesikli uyarı çizgileri çizer hem de sistem terminaline uyarı düşer.
* **Kanal Özelleştirme:** Kanallara anlık olarak özel isimler (`Sıcaklık`, `Basınç`, `Hız` vb.) verilebilir; bu isimler grafik efsanesine, istatistik paneline ve CSV başlıklarına otomatik olarak yansır.
* **Geçmişi Oynatma (Replay Mode):** Daha önce kaydedilmiş `.csv` dosyalarını seçerek sanki donanım yeniden bağlıymış gibi testleri simüle edebilir ve tekrar inceleyebilirsiniz.
* **Profesyonel Veri Kaydı (CSV):** Zaman damgalı, başlıklı ve alarm durumlarını içeren `.csv` formatında otomatik loglama yapar.
* **Crosshair (Koordinat Okuyucu):** Fare imlecini grafik üzerinde gezdirerek herhangi bir noktadaki X ve Y değerlerini nokta atışı inceleyebilirsiniz.
* **Grafik Kontrolleri:** Grafiği dondurma (`Pause`), PNG formatında yüksek çözünürlüklü dışa aktarma ve `Grafiği Ortala` (`Auto-Range`) özellikleri.

---

## 🛠️ Veri Formatı ve Mikrodenetleyiciden Veri Gönderimi

Arayüzün verileri hatasız okuyabilmesi için mikrodenetleyicinizin UART üzerinden belirli bir formatta veri akışı sağlaması gerekmektedir.

### 1. Desteklenen Veri Formatları ve Ayraçlar (Delimiters)
Arayüz üç farklı veri ayracını destekler:
* **Virgül (`,`)** -> Örn: `24.50,1013.25,45.2`
* **Noktalı Virgül (`;`)** -> Örn: `24.50;1013.25;45.2`
* **Boşluk (`Space`)** -> Örn: `24.50 1013.25 45.2`

### 2. Ön Ek (Prefix) Kullanımı
Mikrodenetleyiciler genellikle hem seri porta debug mesajları ("Sistem başlatıldı...") hem de sensör verileri basar. Arayüzün sadece verileri okuması için **Ön Ek** filtrelemesi kullanabilirsiniz. 
* Eğer verilerinizin başında özel bir etiket varsa (Örn: `DATA:24.5,1013`), arayüzdeki Ön Ek kutusuna **`DATA:`** yazarak diğer yazıların grafiği bozmasını engellersiniz. Özel etiket yoksa bu kutuyu boş bırakabilirsiniz.

### 3. Örnek Mikrodenetleyici Kodu (STM32 / C)
Aşağıdaki örnekte `snprintf` kullanılarak güvenli bir şekilde veriler araya virgül konularak ve sonuna satır sonu (`\r\n`) eklenerek gönderilmektedir:

```c
float dalga1 = sin(t) * 50.0f;
float dalga2 = cos(t) * 30.0f + 20.0f;
float gurultu = (rand() % 100) / 10.0f;

// Veriyi araya virgül koyarak ve sonuna \r\n ekleyerek hazırla
int len = snprintf(tx_buffer, sizeof(tx_buffer), "%.2f,%.2f,%.2f\r\n", dalga1, dalga2, gurultu);

// UART üzerinden bilgisayara gönder (Örn: USART2)
HAL_UART_Transmit(&huart2, (uint8_t*)tx_buffer, len, HAL_MAX_DELAY);

t += 0.1f;
HAL_Delay(50); // Saniyede 20 örnek (20 Hz)





Adım Adım Kullanım Kılavuzu
Bağlantı ve Filtreleme Kurulumu:

Cihazınızı bilgisayarın USB portuna bağlayın.

Arayüzden ilgili COM Portunu seçin (Port görünmüyorsa 🔄 Yenile butonuna basın).

Cihazınızın baudrate hızını seçin (Örn: 115200).

Gerekiyorsa Ön Ek (Prefix) bilgisini girin ve veri Ayraç tipini (Virgül, Noktalı Virgül veya Boşluk) seçip "Cihaza Bağlan" butonuna tıklayın.

Kanal İsimlendirme:

✏ İsimleri Değiştir butonuna tıklayarak kanallara kendi özel test isimlerinizi verebilirsiniz.

Veri Kaydı ve Oynatma:

⏺ Kayıt Başlat (CSV) tuşuyla canlı verileri zaman damgalı olarak diske kaydedebilir, ▶ Geçmişi Oynat tuşuyla eski kayıtları tekrar ekrana yansıtıp inceleyebilirsiniz.


Kurulum ve Gereksinimler
Projeyi kaynak kodundan çalıştırmak için gerekli Python kütüphaneleri:
pip install PyQt5 pyqtgraph pyserial
python Akilli_Donanim_Test_İstasyonu.py



Geliştirici: Gani Ahmet Karabacak
