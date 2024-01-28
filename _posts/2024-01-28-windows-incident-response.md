#### Üç Önemli Soru 

#### Incident Response On Windows 

**Hacklenen veya hacklendiğine inanılan bir sistemi analiz ederken, işleme sistemi ne olursa olsun cevaplanması gereken 3 soru vardır. Bu sorulara verilecek yanıtlar değişebilir veya analizin devamını sonlandırabilir.**

- 1. **Sistemde aktif olarak bulunan bir kötü amaçlı yazılım var mı?**
- 2. **Herhangi bir şüpheli iç veya dış iletişim var mı?**
- 3. **Kalıcılık Var mı ?**

#### 1. **Sistemde aktif olarak bulunan bir kötü amaçlı yazılım var mı?**

**Sistemde aktif olarak çalışan kötü amaçlı bir şey varsa geriye doğru analiz yaparak bunun oraya nasıl geldiğini araştırabilirsiniz. Bunu yapmanın en kolay yolu sistem üzerindeki processlerin analizini yapmaktır. Örneğin “excel.exe” işlemi altında yer alan “powershell.exe” alt işlemi şüphelidir ve araştırılması gerekir.**

#### 2. **Herhangi bir şüpheli iç veya dış iletişim var mı?**

**Saldırganın sistemi kontrol etmek veya sistemden veri çıkarmak gibi prosedürleri tamamlayabilmesi için sunucuyla etkileşim kurması gerekir. Bu etkileşim ağ trafiğini oluşturacaktır. O sistemde halihazırda ve geçmişte yapılan bağlantılar analiz edilerek bir anormallik tespiti yapılabilir. Örneğin, kötü şöhrete sahip bir IP ile bağlantı kurulması, belirli bir IP arasında büyük GB hızlarında veri trafiği olması veya anormal portlar arasında bağlantı yapılması dikkatle araştırılması gereken durumlar olabilir.**

#### 3. **Kalıcılık Var mı ?**

**Saldırganın bu güne kadar yaptığı eylemlere bakıldığında, saldırganın ele geçirilen sistemde kalıcı olarak var olmayı hedeflediği açıkça görülmektedir. Bunun nedeni, saldırganın belirli bir işlemi hızlı bir şekilde tamamlayamayıp daha sonra tamamlamak için geri dönmesi gerekebileceği ve ihtiyacı olabileceği için açık bir kapı bırakması gerektiği düşüncesi olabilir. gelecekte tekrar.**

**Analiziniz sırasında aktif bir kötü amaçlı varlığı veya şüpheli trafiği tespit edemeyebilirsiniz. Belki saldırgan haftada bir kez kendini tetikleyebilecek bir arka kapı tutmuştur. Bu nedenle kalıcılık için kullanılan prosedürleri bilmeniz ve bunları sistem içerisinde incelemeniz gerekir.**

**Bahsedilen 3 soruyu cevaplamak önemlidir. Bu sorulara verilecek yanıtlar analizin devamını değiştirebilir. Bu sorulara cevap verebilmek için teknik olarak analiz etmemiz gereken bazı yerler var.**

## Windows Incident Response Sürecinde Kullanılabilecek Ücretsiz Araçlar 

**Olay müdahale sürecinde kullanılabilecek çok sayıda ücretsiz araç vardır. Bazı işlemler manuel olarak yapılabilse de süreci hızlandırmak için bu araçları kullanmanız önemlidir çünkü bazı durumlarda zamana karşı yarışıyor olabiliriz. Bazı ücretsiz araçlar**

- Process Hacker
- FullEventLogView
- Autoruns
- LastActivityViewer
- BrowsingHistoryView 

#### Process Hacker 

**Sistemdeki aktif çalışma süreçlerini detaylı olarak analiz etmek için kullanılabilecek bir araçtır. https://processhacker.sourceforge.io/downloads.php adresinden indirilebilir**

#### FullEventLogView

**Windows olay günlüklerini tek bir pencerede toplar. Özellikle saldırı zaman çerçevesi bilindiğinde uygulanacak doğru filtreler hakkında kanıt toplayabilir. https://www.nirsoft.net/utils/full_event_log_view.html adresinden indirilebilir**

#### Autoruns

**Microsoft'un dahili aracıdır. Saldırganın kalıcılık eylemlerini belirlemeye yardımcı olur. https://docs.microsoft.com/en-us/sysinternals/downloads/autoruns adresinden indirilebilir**

#### LastActivityViewer

**Çeşitli kaynaklardan topladığı verilerle cihazlarda gerçekleşen etkinlikleri sıralar. Belirli bir zaman filtresi uygulandığında çok faydalı olabilir. https://www.nirsoft.net/utils/computer_activity_view.html adresinden indirilebilir.**

#### BrowsingHistoryView

**Cihazdaki web arama motorunun geçmişini okur ve tek ekranda gösterir. Kimlik avı ve web istismarı gibi saldırıları belirlemek için kullanılabilir. https://www.nirsoft.net/utils/browsing_history_view.html adresinden indirilebilir.**


## Live Memory Analysis

**Sistemde aktif olarak çalışan kötü amaçlı bir etkinliği tanımlamanın en iyi yolu bellek analizi yapmaktır. Saldırgan(lar) o anda sisteme uzaktan erişim sağlıyorsa, veri çalıyorsa veya herhangi bir şekilde etkileşimde bulunuyorsa buna izin veren bir process vardır. Buna izin veren processi belirlemek için bellek (ram) analizi yapılabilir.**

**Bellek analizi process hacker tool'u ile yapılabilir. Önemli olan hangi toolu kullandığımız değil neyi aradığımızı bilmektir. Sistem üzerindeki tüm process'leri inceleyebilmemiz için process hacker tool'unu admin yetkisi ile çalıştırmalıyız.**

![Image](/img/process-hacker.png)

Process Hacker aracı sistemdeki süreçlere ilişkin oldukça detaylı veriler sunar. Yukarıda süreç ilişkilerini, PID numaralarını ve çalışan kullanıcı bilgilerini en temel haliyle görebilirsiniz.

Bellek analizi yaparken dikkat etmemiz gereken 3 kritik nokta var:

- Process tree 
- Network Connections
- Signature Status













