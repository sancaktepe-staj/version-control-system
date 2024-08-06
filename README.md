# version-control-system
Development sürecinde dosyalarımızın durumlarını toplu kaydederek (**snapshot**) yeni şeyler denemeyi, kodda veya donanımda (harddisk yandı) herhangi bir sorunu geri almayı ve bir projede pek çok insanın birlikte aynı anda çalışıp değişikliklerin birleştirilmesini sağlayan bir nevi dosya yönetim sistemidir.

## DVCS Öncesi Sistemler

### Local Version Control Systems
Bir klasörde bir dosyanın pek çok farklı kopyası var V1, V2, V3... şeklinde. Scalable değil, örneğin farklı dosyalarınız var:
* yeni bi özellik getirdiniz    A-V1 => A_V2 | C_V1 => C_V2
* farklı bir fikir geldi        A_V1 => A_V3 | C_V2 => C_V1 | B_V1 => B_V2
hangi dosyanın hangisiyle birlikte kullanılacağının takibini yapmak gerekli ki bu da dosya sayısı arttıkça imkansız hale gelicek.

### Centralized Version Control Systems
Merkezi bir server var, dosyaların tüm versiyonları orada ve administrator kimin hangi dosyaya hangi versiyona ne zaman erişebileceğine kontrolü var, değişiklikler genellikle admin sayesinde genellikle farklı dosyalardan olduğu için birleştirme sorunları olmuyor.

Sorun tek bir servera bağımlı olduğundan o serverda bir sorun olması durumunda herkesin elinde tek bir snapshot var ve kaydetmeleri mümkün değil, daha büyük sorun serverda dosyaların corrupt olması durumunda yedekleme yapılmadıysa her şey kayboluyor.a

## Distributed Version Control Systems
Kullanıcılar bir projeden bir dosyanın snapshot'ını değil tüm projeyi alıyorlar serverda herhangi bir sorun bir kullanıcının dosyalarını yüklemesi ile çözümlenebiliyor.

## Git
- Linus Torvalds BitKeeper'dan esinlenerek çıkarttığı VCS aracı.
- Git versiyonlarını "snapshot" şeklinde kaydedildiğini ifade ediyor, delta (difference) ile snapshot arasındaki farkı bilmiyorum, anlayamadım. Sonuçta farklı bir VCS'te A_delta1 ve A_delta2 ayrı şeyler olarak değerlendirilmiyor ki sadece farklar kaydediliyor ve projenin versiyonlarında değişiklik olan her dosya projenin o versiyonuna bağlanarak kaydediliyor? 
- Her operasyon yerel yapılıyor push'lanana kadar
- Dosyaların Hash kodu var böylece dosya ismi önemli değil

### Kullanımı
**Authentication**
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

#### git'te dosyaların 1 + 1 * (3) = 4 durumu vardır: untracked | tracked (unmodified, modified, staged)

git'in o dosyadan haberi varsa tracked yoksa untracked
haberi olan dosyada değişiklik yaptıysan modified
untracked dosyayı aşağıdaki komut ile eklersen
git add a.xyz       // linux terminal gibi çalışır örneğin *.xyz xyz uzantılı tüm dosyalar veya [],? vb.
o dosya modified oluyor // test edelim şuanki halini commitliyorum
commit'lendiği zaman da unmodified'a geri dönüyorlar
