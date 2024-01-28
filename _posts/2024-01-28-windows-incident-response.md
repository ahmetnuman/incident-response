#### **Incident Response On Windows** 

#### **Üç Önemli Soru**

Hacklenen veya hacklendiğine inanılan bir sistemi analiz ederken, İşletim sistemi ne olursa olsun cevaplanması gereken 3 soru vardır. Bu sorulara verilecek yanıtlar değişebilir veya analizin devamını sonlandırabilir.

- **1 Sistemde aktif olarak bulunan bir kötü amaçlı yazılım var mı?**
- **2. Herhangi bir şüpheli iç veya dış iletişim var mı?**
- **3. Kalıcılık Var mı ?**

#### **1 Sistemde aktif olarak bulunan bir kötü amaçlı yazılım var mı?**

Sistemde aktif olarak çalışan kötü amaçlı bir şey varsa geriye doğru analiz yaparak bunun oraya nasıl geldiğini araştırabilirsiniz. Bunu yapmanın en kolay yolu sistem üzerindeki processlerin analizini yapmaktır. Örneğin “excel.exe” işlemi altında yer alan “powershell.exe” alt işlemi şüphelidir ve araştırılması gerekir.

#### **2. Herhangi bir şüpheli iç veya dış iletişim var mı?**

Saldırganın sistemi kontrol etmek veya sistemden veri çıkarmak gibi prosedürleri tamamlayabilmesi için sunucuyla etkileşim kurması gerekir. Bu etkileşim ağ trafiğini oluşturacaktır. O sistemde halihazırda ve geçmişte yapılan bağlantılar analiz edilerek bir anormallik tespiti yapılabilir. Örneğin, kötü şöhrete sahip bir IP ile bağlantı kurulması, belirli bir IP arasında büyük GB hızlarında veri trafiği olması veya anormal portlar arasında bağlantı yapılması dikkatle araştırılması gereken durumlar olabilir.

#### **3. Kalıcılık Var mı ?**

Saldırganın bu güne kadar yaptığı eylemlere bakıldığında, saldırganın ele geçirilen sistemde kalıcı olarak var olmayı hedeflediği açıkça görülmektedir. Bunun nedeni, saldırganın belirli bir işlemi hızlı bir şekilde tamamlayamayıp daha sonra tamamlamak için geri dönmesi gerekebileceği ve ihtiyacı olabileceği için açık bir kapı bırakması gerektiği düşüncesi olabilir. gelecekte tekrar.

Analiziniz sırasında aktif bir kötü amaçlı varlığı veya şüpheli trafiği tespit edemeyebilirsiniz. Belki saldırgan haftada bir kez kendini tetikleyebilecek bir arka kapı tutmuştur. Bu nedenle kalıcılık için kullanılan prosedürleri bilmeniz ve bunları sistem içerisinde incelemeniz gerekir.

Bahsedilen 3 soruyu cevaplamak önemlidir. Bu sorulara verilecek yanıtlar analizin devamını değiştirebilir. Bu sorulara cevap verebilmek için teknik olarak analiz etmemiz gereken bazı yerler var.

## **Windows Incident Response Sürecinde Kullanılabilecek Ücretsiz Araçlar** 

Olay müdahale sürecinde kullanılabilecek çok sayıda ücretsiz araç vardır. Bazı işlemler manuel olarak yapılabilse de süreci hızlandırmak için bu araçları kullanmanız önemlidir çünkü bazı durumlarda zamana karşı yarışıyor olabiliriz. Bazı ücretsiz araçlar

- Process Hacker
- FullEventLogView
- Autoruns
- LastActivityViewer
- BrowsingHistoryView 

#### **Process Hacker** 

Sistemdeki aktif çalışma süreçlerini detaylı olarak analiz etmek için kullanılabilecek bir araçtır. https://processhacker.sourceforge.io/downloads.php adresinden indirilebilir

#### **FullEventLogView**

Windows olay günlüklerini tek bir pencerede toplar. Özellikle saldırı zaman çerçevesi bilindiğinde uygulanacak doğru filtreler hakkında kanıt toplayabilir. https://www.nirsoft.net/utils/full_event_log_view.html adresinden indirilebilir

#### **Autoruns**

Microsoft'un dahili aracıdır. Saldırganın kalıcılık eylemlerini belirlemeye yardımcı olur. https://docs.microsoft.com/en-us/sysinternals/downloads/autoruns adresinden indirilebilir

#### **LastActivityViewer**

Çeşitli kaynaklardan topladığı verilerle cihazlarda gerçekleşen etkinlikleri sıralar. Belirli bir zaman filtresi uygulandığında çok faydalı olabilir. https://www.nirsoft.net/utils/computer_activity_view.html adresinden indirilebilir.

#### **BrowsingHistoryView**

Cihazdaki web arama motorunun geçmişini okur ve tek ekranda gösterir. Kimlik avı ve web istismarı gibi saldırıları belirlemek için kullanılabilir. https://www.nirsoft.net/utils/browsing_history_view.html adresinden indirilebilir.


## **Live Memory Analysis**

Sistemde aktif olarak çalışan kötü amaçlı bir etkinliği tanımlamanın en iyi yolu bellek analizi yapmaktır. Saldırgan(lar) o anda sisteme uzaktan erişim sağlıyorsa, veri çalıyorsa veya herhangi bir şekilde etkileşimde bulunuyorsa buna izin veren bir process vardır. Buna izin veren processi belirlemek için bellek (ram) analizi yapılabilir.

Bellek analizi process hacker tool'u ile yapılabilir. Önemli olan hangi toolu kullandığımız değil neyi aradığımızı bilmektir. Sistem üzerindeki tüm process'leri inceleyebilmemiz için process hacker tool'unu admin yetkisi ile çalıştırmalıyız.

![Image](/img/process-hacker.png)

Process Hacker aracı sistemdeki süreçlere ilişkin oldukça detaylı veriler sunar. Yukarıda süreç ilişkilerini, PID numaralarını ve çalışan kullanıcı bilgilerini en temel haliyle görebilirsiniz.

Bellek analizi yaparken dikkat etmemiz gereken 3 kritik nokta var:

- Process tree 
- Network Connections
- Signature Status

#### **Process Tree**

Bellek analizi yaparken normal durumların ne olduğunu bilmek önemlidir. Örneğin “chrome.exe” işleminin altında “chrome.exe” adında bir childprocess olması normaldir çünkü farklı sekmeler için farklı alt işlemler oluşturabilir.

![Image](/img/chromeexe.png)

Peki ya “chrome.exe” işlemi altında oluşturulmuş bir “powershell.exe” işlemi görsek? Chrome işlemi altında bir PowerShell oluşturmaya normal şekilde tepki veremeyiz. Bir istismar durumundan şüphelenmeli ve PowerShell'in ne yaptığını, hangi komutları davet ettiğini incelemeliyiz.

#### Örnek 1 : WebShell Tespiti

Aşağıdaki process tree'ye bir göz atalım. Web sunucusunun sahip olduğu bir işlem altında bir “powershell.exe: alt işlemi oluşturuldu. PowerShell yerine “cmd.exe” olabilirdi. Ardından “whoami” ve “net user” komutu çalıştırıldı. Olağanüstü durumlar dışında bir PowerShell'in web sunucusu işlemi altında çalışmasını bekleyemeyiz. Ayrıca bunun üzerinde de herhangi bir enumeration komutunun çalıştırılmasını bekleyemeyiz.

![Image](/img/webshell.png)

Bu durumda şu sonuca varabiliriz: Bir web sunucusu işlemi altında bir cmd veya PowerShell işlemi oluşturulmuşsa, bir webshell'den şüphelenmeli ve onu araştırmalıyız.

#### Örnek 1 : Kötü Amaçlı Makro Tespiti

“Winword.exe” işlemini düşünelim. Bir word belgesi açıldığında oluşturulduğunu biliyoruz. Winword.exe işlemi altında powershell.exe dosyasının oluşması normal midir? Peki ya bu PowerShell aslında base64 ile kodlanmış bir komutla çalıştırılıyorsa? Bu durum normal değildir ve büyük ihtimalle içinde kötü amaçlı makro bulunan bir dosyanın açılmasından kaynaklanmaktadır.

![Image](/img/winword.png)

#### Process Hacker Tool'u İle Kontrol

Bir process tree'den kaynaklı şüpheli aktiviteyi nasıl tespit edebileceğimizi teorik olarak çeşitli örneklerle anlattık. Bunları gerçek bir makinede nasıl kontrol edebiliriz?

![Image](/img/processhacker.png)

Yukarıdaki duruma baktığımızda cmd.exe altında python.exe'nin oluştuğunu görebiliriz. Bu durum yasal olabilir ancak aynı zamanda kötü amaçlı bir python komutu çalıştırmış da olabilir. Bunu anlayabilmemiz için “python.exe” dosyasına çift tıklayıp hangi dosyanın/komutun hangi parametrelerle çalıştırıldığını kontrol etmemiz gerekiyor.

![Image](/img/pythonexe.png)

“Komut satırı” alanına baktığımızda, Manage.py dosyasının “runserver” parametreleri içerisinde çalıştırıldığını, “geçerli dizin”de ise işlemin nerede yapıldığını görebiliriz. Burada kesinlikle şüpheli bir durum olduğunu söyleyemeyiz. Durumun şüpheli mi yoksa kötü niyetli mi olduğunu anlayabilmek için “manage.py” dosyasını analiz etmemiz gerekiyor.

## Hands On Lab : 

**Soru : Bir dosya “letsdefend.io” adresine sürekli http request'i göndermesine neden oluyor. Bellek analizi yaparak bu ilgili sürecin ana sürecini bulun.**

Çözüm : nslookup yapıp letsdefend.io ip adresini çözelim.

![Image](/img/nslookup1.png)

Process Hacker tool'unu çalıştırıp , bu ip adreslerinden herhangi birisine istek atan process var mı diye kontrol edelim. Process hacker'ın Network tabında hangi process'in hangi ip adresi ile iletişim kurdurğu incelenebilir. powershell.exe process'ine dikkat !!

![Image](/img/processhack.png)

cmd.exe process'i powershell.exe process'ini çalıştırmaktadır.

![Image](/img/processhack.png)

## **Netwrok Connections**

Saldırgan(lar)ın cihaza uzaktan erişip verileri çalması için arka kapıdan çıkması gerekiyor. Bu arka kapıyı tespit edebilmek için analizimiz sırasında aktif ağ bağlantılarını kontrol etmemiz gerekmektedir. Burada dikkat etmemiz gereken konular proses adı, uzak IP adresi ve port numarasıdır.

Process Hacker’da “Network” sekmesini açarak aktif ağ bağlantısını oluşturan işlemleri görebiliriz.

![Image](/img/netconn.png)

“Uzak adres” kısmında hangi prosesin hangi adresle iletişim kurduğunu ve “Uzak Port” üzerinden hangi porta bağlandığını görebiliriz. Burada ilk kontrol etmemiz gereken şey bağlantı kurmayı beklediğimiz bir sürecimizin olup olmadığıdır. Yukarıdaki görsele baktığımızda Chrome, Discord ve Evernote uygulamalarının ağ bağlantısı kurduğunu görüyoruz. Port bilgilerine baktığımızda bağlantının 443 numaralı port üzerinden yapıldığını anlıyoruz. Yaptığımız ilk analizde herhangi bir doğal olmayan süreç ya da yaygın olarak kullanılmayan bir port tespit edemedik. Eğer şüphelerimiz devam ediyorsa VirusTotal, AbuseIPDB gibi saygın kaynaklarda IP adresi ile ilgili araştırma yapabiliriz.

#### **Signature Status**

Şüpheli olayları tanımlamanın bir diğer yolu da hizmeti çalıştıran dosyanın imzalı olup olmadığını kontrol etmektir. Bir dosyanın imzalanmış olması her zaman onun yasal olduğu anlamına gelmez. Bunun en belirgin örneği yakın zamanda yaşanan SolarWinds olayıdır. SolarWinds olayında saldırganlar, yazılım yayınlanmadan önce kaynak kodunu değiştirmiş ve ilgili birimler zararlı koda imza atmıştı. Böylece SolarWinds'e ait gibi görünen ama aslında kötü amaçlı yazılım olan bir yazılım kamuoyuna dağıtıldı.

Process Hacker'da “Process” bölümünü açın ve hemen altındaki “Name” bölümüne sağ tıklayın ve “Choose columns” seçeneğine tıklayın. Açılan pencerede “Active Columns” bölümüne “Verification Status” ve “Verified Signer” seçeneklerini gönderin ve Tamam’a tıklayın. Böylece aktif olarak çalışan işlemlere ait dosyaların imza durumlarını ve kimler tarafından imzalandığını görebileceksiniz.

![Image](/img/sign1.png)

![Image](/img/sign2.png)

#### **Ne Öğrendik?**

- Bellek analizi sırasında “İşlem Ağacı”, “Ağ Bağlantıları” ve “İmza Durumu” durumlarına dikkat etmeliyiz.
- Canlı Bellek Analizi için Process Hacker aracını nasıl kullanmalıyız?
- Normal ve şüpheli durumları nasıl ayırt edebiliriz?















































