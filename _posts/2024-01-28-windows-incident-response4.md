## Services

Saldırganlar, kötü amaçlı komutları çalıştırmak için yeni bir hizmet oluşturabilir veya mevcut bir hizmeti değiştirebilir. Oluşturdukları veya değiştirdikleri hizmetin tespitini zorlaştırmak amacıyla “Chrome Update” gibi yasal kod adlarını kullanabilirler. Event Log'lardan yeni oluşturulan bir servisin tespiti için “4697: A service was installed in the system” ID'li log kullanılabilir.

Kalıcılığın yanı sıra, hackleme faaliyetlerinin kolayca gerçekleştirilebilmesi için güvenlik önlemi amacıyla çalıştırılan “Windows Defender”, “Firewall” vb. hizmetleri de sürekli olarak durdururlar.

Bu sebeplerden dolayı Windows cihazı analiz ederken hangi servislerin oluşturulduğunu/değiştirildiğini, hangi sistemlerin durdurulduğunu incelememiz gerekiyor.

## Registry Run Keys / Startup Folder

Kullanılan bir diğer önemli yöntem ise “Registry” değerleriyle oynamak veya “Startup” dosyasında bir dosya bırakmaktır. Böylece kullanıcı oturum açtığında istenen dosyanın çalıştırılması sağlanır.

MITRE'nin yaptığı araştırmaya göre APT gruplarının kullandığı 100'den fazla kötü amaçlı yazılım bu tekniği kullanıyor.

#### Startup

Başlangıç ​​dosyasına eklenen dosyaları görebilmek için aşağıdaki indekslerin kontrol edilmesi gerekmektedir.

- C:\Users\Username\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
- C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

![Image](/img/st1.png)

#### Registry Run Keys 

Aşağıdaki Registry Keys Windows sistemlerinde varsayılan olarak oluşturulur:

- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce

Aşağıdaki Registry Keys, başlangıç ​​klasörü öğelerini kalıcılığa göre ayarlamak için kullanılabilir:

- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\UserShellFolders
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\ Explorer\ShellFolders
- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellFolders
- HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\UserShellFolders

Aşağıdaki Registry Keys, önyükleme sırasında hizmetlerin otomatik başlatılmasını denetleyebilir:

- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices Once
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices Once
- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunServices
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunServices

Başlangıç ​​programlarını belirtmek için ilke ayarlarını kullanmak, iki Kayıt Defteri anahtarından birinde karşılık gelen değerleri oluşturur

- HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run
- HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer\Run

#### Detection 

Kayıt defteri değerlerini tek tek kontrol etmek istemiyorsanız “Autoruns” aracına dönebilirsiniz.

![Image](/img/st2.png)


Burada “Logon” ve “Explorer” sekmelerini açarak yukarıda bahsettiğimiz kayıt defteri değerlerini görüntüleyebiliriz. "Control Path” bölümlerini kontrol ederek şüpheli bir dosya olup olmadığını kontrol edebiliriz. Eğer önümüzde çok sayıda kayıt defteri değeri varsa, zamandan tasarruf etmek için “Description” ve “Publisher” bölümlerinde herhangi bir değeri olmayan kayıt defteri değerlerini inceleyerek başlayabiliriz.

Otomatik çalıştırmalarla istediğiniz bulguları bulamadıysanız “Event Viewer”a bakabilirsiniz, bir kayıt değeri değiştiğinde “EventID 4657” logu oluşturulur. Security loglarını filtreleyerek analizinize devam edebilirsiniz.

## Ne Öğrendik ?

- Kullanıcı, bir oturum açıldığında istenen dosyayı çalıştırmak için Başlangıç ​​dosyasını veya Kayıt Defteri değerlerini değiştirebilir.
- EventID 4657 ile kayıt defteri değişikliklerini takip edebiliriz
- Autorun kullanarak şüpheli Kayıt Defteri değerlerini tespit edebiliriz