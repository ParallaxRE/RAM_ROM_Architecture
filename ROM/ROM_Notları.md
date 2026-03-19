
<div align="center">

# 💾 ROM – Sadece Okunabilir Bellek

**Tasarım Amacı:** Kalıcı verileri tutmak için tasarlanmıştır; veriler sık sık değiştirilmez.
**Veri Girişi:** Veriler üretim süreci sırasında girilebilir.
**Mikrobilgisayar Programları:** Uçucu (volatile) olmadığı için mikrobilgisayar programlarını (firmware) saklar.
**Kullanım Alanları:** Hesap makineleri, beyaz eşyalar, güvenlik sistemleri vb. cihazlarda programlanmış halde bulunur.

</div>

---

<img width="653" height="660" alt="resim" src="https://github.com/user-attachments/assets/ae6ba87d-2370-4008-a096-1a61276cc80a" />

> [!NOTE]
> ## 🏗️ ROM Mimarisi (ROM Architecture)
> Temel bir ROM mimarisi, donanım seviyesinde **4 ana parçadan** oluşur. Örnek bir mimaride, 16 farklı adresin her birinde 8-bit veri tutabilen **16 x 8 kapasiteli** bir yapı düşünülebilir.

### 🟢 1. Satır Çözücü (Row Decoder)
* **Görevi:** İşlemciden gelen adres bilgilerinin bir kısmını okuyarak hedef satırı aktif hale getirir.
* **Şema Detayı:** `A0` ve `A1` girişlerini alan **1-of-4 decoder** (4'ten 1'e çözücü) mantığı kullanılır. Bu çözücü, gelen 2 bitlik adrese göre 4 satırdan (Row 0, 1, 2, 3) sadece birini seçer.

### 🟢 2. Sütun Çözücü (Column Decoder)
* **Görevi:** Adres bilgisinin geri kalan kısmını okuyarak doğru sütunu seçer ve sadece o sütundaki verinin veri yoluna (Data bus) inmesine izin verir.
* **Şema Detayı:** `A2` ve `A3` girişlerini kullanarak yine **1-of-4 decoder** mantığıyla 4 sütundan (Column 0, 1, 2, 3) birini hedefler.

### 🟢 3. Register Dizisi (Register Array)
* **Görevi:** Verilerin asıl depolandığı hücre matrisidir. Satır ve sütun çizgilerinin kesiştiği her noktada bir "Register" bulunur.
* **Şema Detayı:** 4 satır ve 4 sütunun kesişimiyle toplam **16 adet Register** oluşur. Her bir register **8-bit** veri (word) saklar. Bir veriyi okumak için o register'ın hem satırından hem de sütunundan "aktif" (Enable - E) sinyali alması gerekir.

### 🟢 4. Çıkış Tamponları (Output Buffer)
* **Görevi:** Register'dan okunan verinin, çipin dışına (işlemciye veya veri yoluna) güvenle aktarılmasını sağlayan çıkış kapılarıdır.
* **Şema Detayı:** 8-bitlik veri yolu çıkış tamponlarına gelir. Ancak verinin pinlere (`D0` - `D7`) yansıması için **CS (Chip Select)** sinyalinin aktif edilmesi şarttır. Aksi takdirde pinler "High-Z" (Yüksek Empedans) durumunda kalır ve veri yolunu meşgul etmez.

---

> [!NOTE]
> ## 💿 ROM (Read-Only Memory) Çeşitleri
> * **Mask-Programmed ROM:** Üretim aşamasında içine veriler "maskelenerek" basılır. Kullanıcı tarafından asla değiştirilemez. Seri üretim çiplerde kullanılır.
> * **PROM (Programmable ROM):** Üretildiğinde boştur. Özel bir cihazla bir kez programlanabilir. Bir hata yaparsan geri dönüşü yoktur, çip "çöp" olur.
> * **EPROM (Erasable Programmable ROM):** İçindeki veriler ultraviyole (UV) ışık ile silinebilir. Silindikten sonra tekrar programlanabilir.
> * **EEPROM (Electrically Erasable PROM):** Veriler ışık yerine elektriksel sinyallerle silinebilir. Çipi yerinden çıkarmadan içindeki veriyi güncelleyebilirsin.
> * **Flash Memory:** Günümüzde en çok kullandığımız türdür (USB bellekler, SSD'ler). EEPROM'un çok daha hızlı ve bloklar halinde silinebilen gelişmiş bir versiyonudur.

---

## 🔍 Çeşitlerin Detaylı Özellikleri

> [!TIP]
> ### Mask-Programmed ROM Özellikleri
> * **Üretici Tarafından Yazılır:** Programlar doğrudan üretici firma tarafından çipin içine yazılır.
> * **Fotoğrafik Negatif (Maske) Kullanımı:** Elektriksel bağlantıları kontrol etmek için bir "maske" kullanılır. Bu, verinin fiziksel olarak çipin devresine işlenmesidir.
> * **Ekonomik Olması İçin Seri Üretim Şart:** Maliyeti düşürmek için ancak çok yüksek miktarlarda üretim yapıldığında ekonomik hale gelir.
> * **Yeniden Programlanamaz:** Veri sonradan değiştirilemez veya silinemez.
> * **Kullanım Alanı Örneği:** Eski tip monitörlerde (CRT) karakter üreticisi olarak.

<img width="1223" height="894" alt="resim" src="https://github.com/user-attachments/assets/37af7cfa-ad02-4de0-a1c7-f7029a7f1ccd" />

---
> [!WARNING]
> ### PROM (Programmable ROM) Özellikleri
> * **Fusible-link MROM (Sigortalı Bağlantı):** İçinde "sigorta" (fuse) adı verilen binlerce küçük iletken köprü vardır.
> * **Kullanıcı Tarafından Programlanabilir:** Üretici bu çipi boş satar. Kullanıcı, özel bir cihazla (PROM programmer) verisini içine işler.
> * **OTP (One Time Programmable):** Bir sigorta yandığında artık geri dönüşü yoktur.
> * **Silinemez:** Bir kez programlandığında içindeki veriler asla silinemez veya üzerine yazılamaz.

<img width="509" height="465" alt="resim" src="https://github.com/user-attachments/assets/817d0c5c-96e3-44fb-882e-8481d09d665a" />

---
> [!WARNING]
> ### EPROM (Silinebilir Programlanabilir ROM) Özellikleri
> * **Özel Voltaj:** Programlamak için normal çalışma voltajından daha yüksek (Genellikle 10-25V arası) voltaja ihtiyaç duyulur.
> * **Zamanlama:** Veriyi hücrelere işlemek için belirli bir süre gerekir.
> * **Silinebilirlik:** İçindeki veriler **Ultraviyole (UV) ışık** kullanılarak silinebilir. (Üzerinde şeffaf bir kuvars pencere bulunur).
> * **Örnekler:** 2732 (4K x 8) ve 2764 (8K x 8) versiyonları.

<img width="1177" height="594" alt="resim" src="https://github.com/user-attachments/assets/2bf23c08-bab0-44f4-ade6-59a307d5431b" />

---
> [!TIP]
> ### EEPROM (Elektriksel Olarak Silinebilir Programlanabilir ROM)
> * **Elektriksel Silme:** Veriler elektriksel sinyallerle silinebilir. Yüksek voltaj çipin kendi içindeki devreler sayesinde üretilir.
> * **Hızlı Yazma:** Yazma süresi hücre başına yaklaşık 5 mikrosaniyedir (5usec).
> * **Otomatik Silme:** Devreler yazılacak olan hücreyi otomatik olarak siler (üzerine yazma işlemi).
> * **Tarihsel Örnekler:** Intel 2816 ve 2864.

<img width="600" height="321" alt="resim" src="https://github.com/user-attachments/assets/f73c2236-7e26-4e89-af76-42f06b5b183c" />

---
> [!TIP]
> ### Flash Memory (Flash Hafıza) Özellikleri
> * **Daha Yüksek Yoğunluk:** EEPROM'a göre daha küçük alanda çok daha fazla veri (GB’larca) saklayabilir.
> * **Daha Hızlı:** EEPROM’dan çok daha hızlı bir silme/yazma döngüsüne sahiptir.
> * **İki Farklı Silme Modu:** Bulk Erase (Toplu Silme) ve Sector Erase (Sektör Silme).
> * **Yazma Süresi:** Yaklaşık 10 mikrosaniye (10 usec) gibi çok kısa bir sürede tamamlar.

<img width="1118" height="663" alt="resim" src="https://github.com/user-attachments/assets/c9a74e4e-97fb-4182-8553-1fc39832ccbd" />

---

> [!NOTE]
> ## ⚙️ ROM Uygulama Alanları (ROM Applications)
> * **Firmware (Donanım Yazılımı):** Cihaz açıldığı anda (power up) çalışan ilk verileri saklar (Örneğin: BIOS).
> * **Data Table (Veri Tablosu):** Sürekli ihtiyaç duyulan sabit verilerin hızlıca "bakılması" (look-up) için kullanılır (Örn: Trigonometrik tablolar).
> * **Data Converter (Veri Dönüştürücü):** Bir veri tipini başka bir tipe dönüştürmek için kullanılır (Örn: BCD kodunu 7-segment koduna çevirmek).

---

## ⚡ ROM ve DAC ile Fonksiyon Jeneratörü

> [!TIP]
> ### 1. Adresleme Birimi (8-bit Counter)
> * **Görevi:** Sistemin "zamanlayıcısıdır". Her saat darbesiyle (CLK) sayıyı 1 artırır (00→01→02⋯→FF).
> * **Mühendislik Detayı:** Sayıcının hızı, çıkıştaki dalganın frekansını (hızını) belirler. Çok hızlı çalıştırılırsa yüksek frekanslı sinyal alınır.

> [!TIP]
> ### 2. Depolama Birimi (256 x 8 ROM)
> * **Görevi:** Bir dalga formunun (Sinüs, Üçgen vb.) örneklerini barındıran "Look-up Table" (Bakma Tablosu) görevi görür.
> * **Neden ROM?:** Karmaşık hesaplamaları anlık olarak işlemciye yaptırmak çok güç tüketir. Önceden hesaplanmış veriyi ROM'dan okumak saniyede milyonlarca kez yapılabilir.
> * **Kapasite:** 256×8 demek; dalganın 256 farklı noktasındaki yüksekliği, 8-bit hassasiyetle kaydedilmiş demektir.

> [!TIP]
> ### 3. Dönüştürme Birimi (8-bit DAC ve Filtre)
> * **DAC (Digital-to-Analog Converter):** ROM'dan gelen dijital sayıları orantılı bir voltaj seviyesine dönüştürür.
> * **RC Filtre:** DAC çıkışı merdiven basamağı gibidir (keskin köşelidir). Bu filtre, o köşeleri "yumuşatır" ve pürüzsüz bir analog dalga (Sinüs) elde edilmesini sağlar.
