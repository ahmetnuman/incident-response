#### **Incident Response On Windows - Devam** 

## **Users**

Kalıcılığı korumak için saldırganların yaygın olarak kullandığı bir yöntem, kullanıcı oluşturmaktır. Saldırgan(lar)ın “Administrator” hesabının kontrolünü ele geçirdiğinde yeni kullanıcılar oluşturduğunu gözlemliyoruz, çünkü administrator önemli bir kullanıcıdır ve etkinliği düzenli olarak takip edilebilir. Böylece çok fazla dikkat çekmeyecek yeni bir kullanıcı oluştururlar ve mümkünse o kullanıcının ayrıcalıklarını arttırırlar.

Oluşturulan kullanıcılar genellikle “support”, “sysadmin”, “admin” gibi anahtar kelimeleri içerir. Çoğu firmada bu gibi isimlere sahip kullanıcılar pek ilgi görmeyecektir.

Bir olaya müdahale prosedürü sırasında hızlı bir şekilde değerlendirmemiz gereken 2 şey vardır.

- Şu anda sistemde olmaması gereken bir kullanıcı var mı?
- Saldırı sırasında bir kullanıcı oluşturuldu ve sonrasında silindi mi?

#### **Şüpheli Kullanıcı Tespiti**

Sistemde o an aktif olan kullanıcıları listelemek için cmd üzerinden `net user` komutunu kullanabiliriz.

![Image](/img/netuser.png)

Sonuç olarak eğer orada olmaması gereken bir kullanıcı varsa ve bu kullanıcı hakkında daha detaylı bilgiye ihtiyacımız varsa `net user USERNAME` yazarak arama yapabiliriz.

![Image](/img/netusertest.png)

Bu örnekte “Last logon” ve “Password last set” değerleri saldırının yapıldığı zaman ile eşleştirilirse duruma şüpheyle yaklaşabiliriz.

Diğer bir yöntem ise “lusrmgr” üzerinden kontrolü sağlamaktır. Bunun için “Windows + R” ile “run:”u aktif hale getirin ve `lusrmgr.msc` yazarak Tamam’a tıklayın.

![Image](/img/lusr.png)

Açılan pencerede “Users” grubunu seçip kullanıcıları listeleyebilirsiniz.

Bu kontrolden sonra bir kullanıcıdan şüpheleniyorsanız bir sonraki analiz döneminizde o kullanıcının aktivitesine odaklanabilirsiniz.

#### **Geçmişte Oluşturulan Kullanıcılar**

Saldırganlar, kullanıcı oluşturup gerekli işlemleri yaptıktan sonra, geride bıraktıkları izi en aza indirmek için işleri bittiğinde kullanıcıları silebilir. Bu durumda “net user” ya da “lusrmgr” ile yaptığımız komutlarda bu kullanıcıları göremeyeceğiz. Yapmamız gereken “Güvenlik” loglarını kontrol ederek geçmişte bir kullanıcı oluşturulup oluşturulmadığını gözlemlemek. Bunu yapmak için “Event ID 4720 – A user account was created” logunu kullanabiliriz.

“Security” loglarını “Event Viewer” ile açtıktan sonra Event ID “4720” ile logları filtreleyebiliriz.

![Image](/img/evevi.png)

![Image](/img/filterlog.png)

Açılan pencerede Event ID olarak “4720” giriyoruz.

![Image](/img/f2.png)

![Image](/img/f3.png)

Ortaya çıkan sonuca baktığımızda “LetsDefend” kullanıcısının “10/10/2021 10:07” tarihinde “SupportUser” adında bir kullanıcı oluşturduğunu görmekteyiz. Buradan çıkarmamız gereken sonuç şudur; “LetsDefend” kullanıcısı devralınmış veya komutların çalıştırılmasına imkan verecek bir erişim yapılmış. Bu noktadan sonra hem “LetsDefend” hem de “SupportUser” kullanıcılarının aktivitelerinin takip edilmesi gerekmektedir.

Ek olarak , Kullanıcılarla ilgili şüpheli durumları yakalamak için, saldırının gerçekleştiği dönemde “Administrators” grubuna eklenen kullanıcıları sorgulayabilirsiniz. Böylece eklenmemesi gereken bir kullanıcıyı hemen yakalamış olacaksınız. Bunun için aşağıdaki etkinlik kimliğini kullanabilirsiniz.

- Event ID 4732 – A member was added to a security-enabled local group.

#### **Ne Öğrendik?**

- Saldırganlar sistemin kontrolünü ele geçirdikten sonra “support” veya “admin” gibi genel isimler kullanabilirler.
- Yeni kullanıcının aktivitelerinin yanı sıra yeni kullanıcı oluşturan kullanıcının aktivitelerinin de takip edilmesi gerekmektedir.
- Önceki aktiviteleri takip ederken 4720 ve 4732 EventID loglarını kullanmak faydalı olacaktır.

#### **Hands On Lab**

- Soru : 23.10.2021 tarihinde oluşturulan ve "Administrators" grubuna eklenen kullanıcı adı nedir?

- Cevap : Event Viewer ı aç ve o tarihteli 4732 event id'sini araştır.

![Image](/img/f3.png)
































