<div align="center">
💾 RAM – Random Access Memory (Rastgele Erişimli Bellek)

Okunabilir ve Yazılabilir (Read/Writeable): ROM'un aksine, RAM'in içindeki veriler her an değiştirilebilir, silinebilir veya üzerine yeni veri yazılabilir.
Uçucu (Volatile): Elektrik kesildiği anda içindeki tüm veriler silinir. Bu yüzden kalıcı depolama değil, geçici depolama (temporary storage) veya register olarak kullanılır.

</div>

    [!NOTE]
    ⚙️ Teknolojisine Göre 2 Tür

        Static RAM (SRAM): Yarı iletken tabanlıdır. Çok hızlıdır ama pahalıdır. Genellikle işlemci içindeki "Cache" (Önbellek) olarak kullanılır.

        Dynamic RAM (DRAM): Kondansatör (Capacitor) tabanlıdır. Daha yavaştır ama ucuzdur ve yüksek kapasitelerde (8GB, 16GB RAM'ler gibi) üretilebilir.

        Boyut Adımları: Kapasiteleri byte bazından başlar ve 1K, 2K, 4K gibi 2^n katları şeklinde artarak ilerler.

🛠️ RAM OPERASYON ADIMLARI

    [!TIP]
    📥 Ortak Adımlar (Başlangıç)

    Hangi işlemi yaparsan yap, önce şu iki adımı tamamlaman gerekir:

        Adres Kodunu Ayarla (Set Address): İşlemci, hangi hücreye ulaşmak istediğini "Adres Yolu"na (Address Bus) yazar.

        Çipi Aktifleştir (/CS): "Chip Select" sinyali gönderilerek ilgili RAM çipi uyandırılır.

    [!WARNING]
    ✍️ Yazma Döngüsü (Write Cycle) Adımları

        Adres Kurulumu: İşlemci, veriyi nereye yazacağını belirlemek için adres yoluna (Address inputs) bilgiyi gönderir. Diyagramda görülen tAS (Address Setup Time), adresin RAM tarafından algılanması için gereken minimum süredir.

        Yazma Sinyali (W/R): Yazma moduna geçmek için sinyal düşük seviyeye çekilir. Bu, RAM'e "kapıları aç, veri geliyor" komutudur.

        Data Setup (tDS): En önemli kısımdır. Verinin, yazma sinyali sonlanmadan önce veri yolunda hazır ve kararlı hale gelmesi gerekir. Eğer veri bu süreden daha kısa sürede yola çıkarsa, hücreye yanlış veya bozuk bilgi yazılır.

        Data Hold (tDH): Yazma sinyali kesildikten (yüksek seviyeye çıktıktan) sonra verinin hat üzerinde ne kadar daha tutulması gerektiğini ifade eder. Bir nevi "kuruma süresi" gibi düşünebilirsin; sinyal biter bitmez veriyi hattan çekersen hücre içeriği tam dolmadan işlem yarım kalabilir.

    [!TIP]
    📖 Okuma Döngüsü (Read Cycle) Süreci

        Adres Geçerliliği (Address Valid): İşlemci, okumak istediği hücrenin adresini adres yoluna bırakır. Diyagramın en üstünde gördüğün tRC (Read Cycle Time), tüm bu okuma işleminin toplam süresidir.

        Okuma Komutu (R/W): Okuma yapılacağı için bu sinyal genellikle yüksek (1) seviyede tutulur.

        Chip Select (CS) Aktivasyonu: RAM birimine "seninle konuşuyorum" sinyali gönderilir (0'a çekilir). Bu andan itibaren zaman sayacı işlemeye başlar.

        Adres Erişim Süresi (tACC): Bu, okuma döngüsünün en kritik gecikmesidir. Adres verildikten sonra RAM'in içindeki dekoderlerin doğru hücreyi bulup, oradaki veriyi çıkış kapısına (Data Output) iletmesi için gereken süredir.

        Data Valid (Veri Kararlı): tACC süresi dolduğunda, veri çıkış hattındaki o belirsiz (High-Z) durum sona erer ve "Data valid" kutusu oluşur. Artık işlemci bu veriyi güvenle alıp kendi register'larına kopyalayabilir.

🏗️ DRAM Mimari Özeti

Görseldeki yapı, 16K x 1 bitlik bir DRAM'i temsil ediyor. Yani toplamda 16.384 adet hücre var ve her hücre sadece 1 bit saklayabiliyor.

    [!NOTE]
    1. Satır ve Sütun Düzeni (128x128 Matris)

    Tüm bellek hücreleri tek bir uzun sıra yerine, 128 satır ve 128 sütun şeklinde bir kare ızgaraya yerleştirilmiştir.

        Neden? Eğer 16.384 hücreyi yan yana dizseydin, her birine ulaşmak için binlerce kablo (pin) gerekecekti. Matris yapısı sayesinde sadece 128+128 hatla tüm hücrelere erişilebiliyor.

    [!NOTE]
    2. Adres Kod Çözücüler (Decoders)

    Görselin solunda ve üstünde gördüğün bloklar bellek biriminin "navigasyon" sistemidir:

        Row Address Inputs (A0 - A6): 7 bitlik bu adres, "1-of-128 decoder" aracılığıyla 128 satırdan hangisinin aktif edileceğini seçer.

        Column Address Inputs (A7 - A13): Diğer 7 bitlik adres ise yukarıdaki decoder aracılığıyla hangi sütundaki verinin okunacağını belirler.

        Toplam Adres: 7 bit (satır) + 7 bit (sütun) = 14 bit. 214=16.384 adres noktasına tam olarak denk gelir.

🧱 1. Görseldeki Bileşenlerin Görevleri

    Kondansatör (C): Hücrenin kalbidir. Veriyi elektrik yükü olarak saklar. Şarjlıysa "1", boşsa "0" demektir.

    S (Switch) Anahtarları: Veri akışını kontrol eden kapılardır (Sembolik olarak transistörleri temsil ederler).

    Sense Amplifier (Algılama Yükselteci): Kondansatördeki çok zayıf voltajı okur, VREF (Referans Voltajı) ile karşılaştırır ve kararlı bir dijital sinyale (1 veya 0) dönüştürür.

    Data In / Out: Verinin hücreye girdiği ve çıktığı yollardır.

🕹️ 2. Operasyonların Adım Adım İşleyişi

    [!TIP]
    Yazma İşlemi (WRITE)

    Veriyi içeri "hapsettiğimiz" süreçtir.

        S1 ve S2 Kapatılır: Kapılar açılır.

        Şarj Etme: DATA IN hattından gelen voltaj kondansatöre (C) akar.

        Kilitlenme: Anahtarlar tekrar açıldığında, elektrik kondansatörde hapsolur. Veri artık fiziksel olarak oradadır.

    [!IMPORTANT]
    Okuma İşlemi (READ)

    Veriyi kontrol ettiğimiz ama aynı zamanda "tükettiğimiz" süreçtir.

        S2, S3 ve S4 Kapatılır (S1 hariç): Kondansatördeki yük Sense Amplifier'a süzülür.

        Karşılaştırma: Yükseltici, gelen yükü referansla kıyaslar ve DATA OUT'a net bir "1" veya "0" basar.

        Geri Yazma (Refresh): Okuma sırasında kondansatördeki yük dışarı boşaldığı için veri kaybolur (Destructive Read). Bu yüzden S4 üzerinden gelen sinyal, kondansatörü anında tekrar şarj eder.

🔀 Adres Multiplexing

Normalde bellekteki bir veriye ulaşmak için bir adres göndermen gerekir. Bellek kapasitesi arttıkça, bu adresi taşımak için gereken kablo (pin/line) sayısı da artar. Örneğin, çok büyük bir belleğin varsa yüzlerce pin gerekirdi; bu da anakartta yer kaplar ve maliyeti artırır.
Multiplexing ise bu işi "sırayla" yaparak pin sayısını yarıya indirir. Adresi tek seferde değil, iki aşamada göndeririz.

    [!NOTE]
    (A) Mimari Şema: Multiplexing Burada Nerede?

    Şemanın sol tarafına bakarsan, giriş pinlerinin A0/A7, A1/A8... şeklinde isimlendirildiğini görürsün. İşte Multiplexing tam olarak budur:

        Pin Tasarrufu: Bu bellek 128x128=16.384 (16K) hücreden oluşuyor. Normalde her hücreye ulaşmak için 14 adet adres pinine (214=16.384) ihtiyacın olurdu. Ama şemada sadece 7 adet adres giriş hattı var.

        İki Aşamalı Giriş: 14 bitlik adres, 7'şer bitlik iki paket halinde aynı hatlardan sırayla içeri alınır.

            1. Aşama (Satır): İlk 7 bit gelir, RAS sinyaliyle "Row Address Register" (Satır Adres Kaydedicisi) içine kilitlenir.

            2. Aşama (Sütun): İkinci 7 bit gelir, CAS sinyaliyle "Column Address Register" (Sütun Adres Kaydedicisi) içine kilitlenir.

        Decoder (Kod Çözücü): Kaydedicilerdeki bu 7'şer bitlik veriler, "Decoder" birimleri tarafından çözülerek 128 satır ve 128 sütundan oluşan matriste tam koordinatı belirler.

    [!NOTE]
    (B) Zamanlama Diyagramı: Multiplexing Nasıl Yönetilir?

    Multiplexing işleminin "sırasını" bu diyagramdaki sinyaller belirler:

        Address Inputs: Hatlara önce "Row Address" (Satır Adresi) verilir.

        RAS (Row Address Strobe): Bu sinyal aşağı düştüğü an (t1), yonga "Tamam, şu an hatlarda duran 7 biti satır adresi olarak hafızama aldım" der.

        CAS (Column Address Strobe): Hatlardaki veri "Column Address" (Sütun Adresi) ile değiştirildikten sonra bu sinyal aşağı düşer (t3). Yonga şimdi de sütun adresini kabul eder.

    [!WARNING]
    (A) Şeması: Multiplexing Yok (ROM veya SRAM)

    Burada işlemci (CPU), belleğe (ROM veya statik RAM) direkt bağlıdır.

        Mantık: İşlemcinin 14 adet adres pini (A0 - A13) vardır ve belleğin de 14 adet giriş pini vardır.

        İlişki: Adres direkt olarak, tek seferde gönderilir. Multiplexing yoktur.

        Sorun: Bellek kapasitesi arttıkça (örneğin 16GB RAM olduğunu düşün), pin sayısı binlere çıkardı. Bu da fiziksel olarak imkansız bir anakart tasarımı demek olurdu.

    [!TIP]
    (B) Şeması: Multiplexing Var (DRAM)

    İşte burası senin "Address Multiplexing" dediğin yerin tam olarak gerçekleştiği nokta. İşlemci ile DRAM arasına bir Multiplexer (MUX) yerleştirilmiş.

        Multiplexer'ın Rolü: İşlemciden çıkan 14 adet kabloyu alır ve bunları sadece 7 adet kabloya indirger.

        MUX Sinyali (Seçici): Şemanın altındaki nota bakarsan:

            MUX = 0 olduğunda: İlk 7 bit (A0-A6) DRAM'e gönderilir.

            MUX = 1 olduğunda: Diğer 7 bit (A7-A13) aynı yollar üzerinden gönderilir.

        İlişki: Bu sayede DRAM yongası üzerinde 14 pin yerine sadece 7 pin (A0/A7 gibi) kullanılmış olur.

🔄 Refreshing

    Temel Mantık: DRAM hücreleri veriyi minik kapasitörlerde elektrik yükü olarak tutar. Bu kapasitörler zamanla elektrik sızdırdığı için verinin silinmemesi adına periyodik olarak şarj edilmeleri gerekir.

    Periyot (Zamanlama): Her bir bellek hücresi tipik olarak her 4 ms (milisaniye) içinde en az bir kez tazelenmelidir, aksi takdirde içerideki veri (0 veya 1 bilgisi) kaybolur.

    Okuma İşlemi ile İlişki: Bellekten bir veri okunduğunda, o satır otomatik olarak tazelenmiş olur. Ancak okunmayan hücrelerin de unutulmaması için özel bir tazeleme döngüsüne ihtiyaç vardır.

    Donanım Gereksinimi: Bu işlem için bellek denetleyicisi (Memory Controller) içinde veya harici bir devrede ROW (Satır) adres sinyalini aktif edecek ek mekanizmalar bulunur.

    [!NOTE]
    2 Ana Tazeleme Modu

        Burst Mode (Patlama Modu):

            Sistemin normal işleyişi tamamen durdurulur (suspend).

            Tüm bellek satırları arka arkaya, hızlı bir "patlama" şeklinde tazelenir.

            Bu sırada işlemci belleğe erişemediği için kısa süreli performans kayıpları yaşanabilir.

        Distributed Mode (Dağıtılmış Mod):

            Tazeleme işlemi, normal bellek operasyonlarının arasına küçük parçalar halinde serpiştirilir (interspersed).

            Sistem tamamen durmaz; tazeleme ve veri işleme sırayla, karma şekilde yapılır.

            Modern sistemlerde (senin HP Victus'un dahil) akıcılığı bozmadığı için bu mod tercih edilir.

    [!IMPORTANT]
    Tazeleme Sinyal Analizi

        Sinyal Durumu: Tazeleme işlemi sırasında sadece RAS (Satır Adres Seçici) sinyali aktif edilir (voltajı "Low" yani aşağı çekilir).

        Devre Dışı Sinyaller: CAS (Sütun Adres Seçici) ve R/W (Okuma/Yazma) sinyalleri HIGH (yukarıda/pasif) konumda tutulur.

        Mantıksal Sebep: Tazeleme sırasında belirli bir sütuna gitmeye veya yeni veri yazmaya gerek yoktur; sadece satırdaki kapasitörleri şarj etmek (uyandırmak) yeterlidir.

        Adresleme Akışı: Diyagramda görüldüğü gibi, her bir RAS darbesinde adres hattındaki satır numarası sırayla artar (ROW 0, ROW 1, ROW 2 ... ROW 127).

        Enerji Tasarrufu: Sütun (CAS) devreleri çalıştırılmadığı için bu yöntem "Full Access" (Tam Erişim) moduna göre çok daha az güç tüketir.

        Veri Koruma: Bu periyodik döngü sayesinde, DRAM içindeki o mavi gözlü ejderha görseli veya film dosyaların kapasitörlerden sızan elektriğe rağmen silinmeden korunur.

📏 Word Size Expanding

    Temel Amaç: Elimizdeki düşük bit genişliğine sahip RAM modüllerini paralel bağlayarak, işlemcinin ihtiyacı olan daha geniş veri yolunu (Data Bus) oluşturmaktır.

    Modüllerin Yapısı: Şemada iki adet 16 x 4 bitlik RAM kullanılmış. Bu, her bir RAM'in 16 farklı adrese sahip olduğu ama her adreste sadece 4 bit veri tutabildiği anlamına gelir.

    Paralel Bağlantı: İki RAM modülü yan yana getirilerek 16 x 8 bitlik tek bir bellek bloğu elde edilmiştir. Yani bellek kapasitesi (adres sayısı) aynı kalmış, ancak verinin "genişliği" 4'ten 8'e çıkmıştır.

    Ortak Adres Yolu (Address Bus): Adres hatları (AB0 - AB3) her iki RAM modülüne de ortak olarak bağlanmıştır. İşlemci bir adres gönderdiğinde, her iki RAM de aynı anda "uyanır".

    Ayrılmış Veri Yolu (Data Bus): Veri hatları (DB0 - DB7) ikiye bölünmüştür:

        RAM-0: Verinin üst 4 bitini (high-order bits, DB4 - DB7) saklar.

        RAM-1: Verinin alt 4 bitini (low-order bits, DB0 - DB3) saklar.

    Kontrol Sinyalleri: Okuma/Yazma (R/W) ve Chip Select (CS) sinyalleri her iki modüle de aynı anda gider. Böylece işlemci "oku" dediğinde, her iki modül kendi 4 bitini gönderir ve veri hattında toplam 8 bitlik tam bir "word" (kelime) oluşur.

🗄️ Capacity Expanding

    Temel Amaç: Elimizdeki RAM modüllerini kullanarak işlemcinin erişebileceği toplam adres alanını (toplam kelime sayısını) büyütmektir.

    Modüllerin Yapısı: Şemada iki adet 16 x 4 bitlik RAM kullanılmış. Hedef, bunları birleştirerek 32 x 4 bitlik bir bellek yapısı elde etmektir. Dikkat edersen veri genişliği (4 bit) aynı kalıyor, sadece adres sayısı 16'dan 32'ye çıkıyor.

    Ortak Veri Yolu (Data Bus): Bu sefer veri hatları (DB0 - DB3) her iki RAM modülüne de ortak bağlanmıştır. Çünkü aynı anda sadece bir RAM veri gönderecek veya alacaktır.

    Adres Seçimi (The High+1 Line): Toplam 32 adrese ulaşmak için 5 adres hattına (25=32) ihtiyaç vardır.

        İlk 4 hat (AB0 - AB3) her iki RAM'e de ortak gider.

            hat (AB4 - High+1 line) ise hangi RAM'in o an aktif olacağını seçer.

    Chip Select (CS) ve NOT Kapısı: Şemadaki en kritik nokta burasıdır. AB4 hattı doğrudan RAM-0'ın CS ucuna, bir NOT kapısı (invertör) üzerinden ise RAM-1'in CS ucuna gider.

        Eğer AB4 = 0 ise: RAM-0 aktif olur, RAM-1 kapalı kalır. (00000 ile 01111 arası adresler).

        Eğer AB4 = 1 ise: RAM-1 aktif olur, RAM-0 kapalı kalır. (10000 ile 11111 arası adresler).

    Address Ranges (Adres Aralıkları): Şemanın altında belirtildiği gibi; sistem 00000'dan başlar, 11111'e kadar gider. Toplamda 32 farklı "word" (sözcük) kapasitesine ulaşılmış olur.

🧩 Combining Chips

    Temel Amaç: Dört adet 2K x 8 bitlik PROM (Programlanabilir ROM) yongasını birleştirerek toplamda 8K x 8 bitlik tek bir bellek alanı elde etmektir.

    Bellek Yapısı: Veri genişliği (8-bit) her yongada aynıdır. Biz bu yongaları "arka arkaya" dizerek adres derinliğini (kapasiteyi) artırıyoruz.

    Decoder (Kod Çözücü) Kullanımı: İkiden fazla yonga olduğunda, sadece bir NOT kapısı yetmez. Burada 74LS138 (3-line-to-8-line decoder) kullanılmış. Bu entegre, gelen adres sinyallerine göre hangi yonganın aktif olacağını seçer.

    Adres Paylaşımı:

        Toplam 8K adrese ulaşmak için 13 adres hattına (213=8192) ihtiyaç vardır.

        İlk 11 hat (AB0 - AB10), her dört PROM yongasına da ortak gider. Bu hatlar yonganın içindeki spesifik adresi seçer.

        En üstteki 2 hat (AB11 ve AB12), Decoder'ın girişine gider. Bu hatlar "Hangi yonga çalışacak?" kararını verir.

    Yonga Seçimi (Chip Select): Decoder'ın çıkışları (0,1,2,3), her bir PROM yongasının CS (Chip Select) ucuna bağlıdır.

        Eğer AB11 AB12 = 00 ise PROM-0 aktif olur.

        Eğer AB11 AB12 = 01 ise PROM-1 aktif olur ve bu böyle devam eder.

    Ortak Veri Yolu (Data Bus): Tüm yongaların veri çıkışları (O0 - O7), ortak 8-bitlik veri yoluna (DB0 - DB7) bağlıdır. Decoder sayesinde aynı anda sadece bir yonga veri gönderdiği için hatlar çakışmaz.

    Hexadecimal Adres Aralıkları: Şemanın altında gördüğün gibi; her yonga 0800 (hex) birimlik bir yer kaplar.

        PROM-0: 0000 - 07FF arası.

        PROM-3: 1800 - 1FFF arası (toplam 8K kapasiteye ulaşılır).
