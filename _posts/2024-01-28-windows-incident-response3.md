## Task Scheduler (Görev Zamanlayıcısı)

En çok kullanılan kalıcılık yöntemlerinden biri zamanlanmış görevler oluşturmaktır. Virüslerden fidye yazılımlarına kadar çoğu kötü amaçlı şey, kalıcılığı korumak için zamanlanmış görevleri kullanır. Saldırgan, zamanlanmış görevleri kullanarak kötü amaçlı dosyanın düzenli aralıklarla çalışmasını sağlar. Böylece saldırgan çalıştırmak istediği komutların aktif ve düzenli çalışmasını sağlar. Sistemde aktif olarak mevcut şüpheli zamanlanmış görevleri tanımlamanın çeşitli yolları vardır. Öncelikle bunun bir sysinternals aracı olan “Autoruns” kullanılarak nasıl yapıldığını gösterelim.

#### Autoruns

Autoruns aracıyla saldırganların kullandığı Kayıt Defteri (Registry), Başlangıç (Startup) ​​ve Görev Zamanlama (Schedule Task) gibi yaygın olarak kullanılan kalıcılık yöntemlerini tespit edebiliyoruz. Aracı yönetici olarak çalıştırıyoruz ve Scheduled Task” sekmesine gidiyoruz.

![Image](/img/s1.png)

Önümüzde çok fazla planlanmış görev yok. Gerektiğinde her birini ayrı ayrı inceleyip şüpheli olanı tespit edebiliyoruz. Ancak planlanmış görev sayımız fazlaysa ve zamana karşı yarışıyorsak ne yapabileceğimizi düşünelim. İlk elemeyi gerçekleştirmek için “Publisher” ı olmayan, zamanlanmış görevlerle başlayabiliriz. Publisher olması onu tamamen güvenilir kılmaz ancak şüpheli görevlerin yayıncısının olmaması daha yüksek bir ihtimaldir.

![Image](/img/s2.png)

Artık sayı 3’e düştü. Şimdi yapmamız gereken, zamanı geldiğinde çalışacak olan “image Path” içerisinde yer alan dosyayı analiz etmek.

“Update-Daily” görevi için “important.bat” dosyası incelendiğinde aşağıdaki komutların çalıştırıldığını görebiliriz.

![Image](/img/s3.png)

Böylece saldırganın asıl amacının, RDP çalıştırabilmesi için “User123” adında bir kullanıcı oluşturup ilgili gruba eklemek olduğunu görüyoruz. Saldırgan bunu elle değil, zamanlanmış bir görevle yapmayı tercih etti.

## Task Scheduler

Otomatik çalıştırma aracını indirmek istemeyenler, şüpheli görevleri tespit etmek için işletim sisteminde bulunan varsayılan "Görev Zamanlayıcı"yı kullanabilir.

![Image](/img/s4.png)

Burada otomatik çalıştırmalara benzer herhangi bir ek bilgi sunulmasa da ilgili göreve tıkladığınızda hangi dosya veya komutun çalıştırıldığını görebilir ve analiz edebilirsiniz.

![Image](/img/s5.png)

Her ne kadar çok tercih edilmese de arayüz kullanma şansınız yoksa komut satırı arayüzünde yer alan 'schtasks' komutu ile zamanlanmış görevleri görüntüleyebilirsiniz.

![Image](/img/s6.png)

















