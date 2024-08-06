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
o dosya staged oluyor

test edelim şuanki halini commitliyorum         bf3d67fd926ba3f0792926cf745d0eea5b892ae5
yeni dosya oluşturduk text.txt
    git status
untracked olduğunu gördük
    git add .
staged oldu
    git commit -m "untracked file commit"       436cbdff2d62c21a95d3fb7d1124975613ee59c8
commit'lendiği zaman da unmodified'a geri dönüyorlar

unstaged dosyaları gözlemlemek için
    git diff
unstaged dosyalarınızı stagelemekle uğraşmamak için -a seçeneği mevcut

#### .gitignore
belki bazı dosyalarımızı sadece kendimiz için tutuyoruz veya bilmiyorum android için yazdığımız bir programı windows'da yazarken testing için kullandığımız emulator kendi dosyalarını eklemiştir repository'e... bilmiyorum

bu gibi durumlarda git "gereksiz" diyebileceğim dosyaların snapshot'a eklenmemesi için .gitignore'a o dosyaları ekleyebiliriz.
    unity/*Fun*

bir dosyayı gitten silmek için önce dosya ekleyip commitleyelim
    git add .
    git commit -m "test2 added2"
test2.txt yi silelim
    git status
iki seçenek var ya direkt sildiğimiz söyleyeceğiz
    git rm test2.txt
veya
    git add .

#### geçmiş commitleri inceleme
birazdan branchlere geçeceğiz ondan önce mevcut branch'teki commit'leri yönetelim
hangi branch'te olduğumuzu
    git status
commitlerimizi görmek için
    git log     // powershell'deysek log'dan çıkmak için "q"
pek çok farklı option var log'da çünkü yüzlerce commiti olacaktır aylarca uğralılan bir projede ihtiyacınıza göre bakın optionlara

en son açtığınız commit veya stage den sonra bir dosyada yaptığınız değişiklikleri geri almak için
    git checkout -- filename.xyz
stageleseniz bile geri almak için
    git restore filename.xyz

checkout un "asıl görevi" bu değil geçmiş commitlerde gezinmek için ki aslında bunu branchlerle daha kullanışlı hale getireceğiz extensionlar ile bu işlemleri terminal yerine UI üzerinden de gerçekleştirebilirdik
    git add .
    git commit -m "checkout yapacagım"
    git push
    git log
test.txt yi eklemeden önceki bir halimize gidelim
    git checkout 5733a3ae01d6edd6c8f3f89dbb42db8310d421d2
aynı komutla geri dönebiliriz
    git checkout main

#### Remote
Yaptığımız her şeyi remote repository lerde de yapabiliriz read/write (pull/push) olması kaydı ile
    git remote
origin göreceğiz, git in servera atadağı default isim
    git fetch origin
yaptığımızda serverdaki değişiklikler bizim git imize eklenicek biz ulaşmadığımız sürece local repository de görünmeyecek
    git pull
yapsaydık son hali local repository e gelicekti
    git push ile değişikliklerimizi servera yüklüyoruz conflict olursa onunla ilgileniyoruz conflict i daha sonra konuşalım

#### Tag
versiyon belirtebiliriz belirtelim -a ile ekliyoruz option olmadan görüntülüyoruz
    git tag
boş
    git tag -a v0.1 -m "bu durumda bişey ifade etmiyor"
    git tag
v0.1
taglerin güzel kısmı checkout için hash yerine tag yazabiliriz
    git add .
    git commit -m "checkout tag"
    git tag -a v0.2
    git checkout v0.1
    git checkout v0.2

#### Branches




