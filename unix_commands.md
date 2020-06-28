# Important Unix/Shell commands

## Şu ana kadar kullandığım ve önemli bulduğum linux komutlarının listesi

---

### Genel Bilgilendirme

* `sudo su` = root şifresi tanımlama
* `.` = bulunduğun dizin
* `~` = Home
* `~/.bashrc` = kullanıcı ile ilgili kofigürasyonların tutulduğu klasör
* . ile başlayan dosyalar = gizli dosyalar
* `cd Enter` = go home directory
* `!` = ardına yazılan komut, geçmişte yazılmışsa en son yazılanını çalıştırır (örnek : !curl)
* `ls -lh` = dizindeki dosyaları boyutları anlaşılabilecek şekilde listeler
* `du -h` = dizindeki klasörleri boyutları anlaşılabilecek şekilde listeler
* `df -h` = harddisk kullanım bilgilerini listeler
* `lsb_release -a` = işletim sistemi ve distribution ile ilgili bilgileri verir
* `cat /etc/*release` = işletim sistemi bilgilerini gösterir
* `lsof -nP | grep '(deleted)'` = silinen ama linkleri kopmayan dosyaları listeler

---

### İki dosya metnindeki farkları bulma komutu

> `diff /dosyaYolu1 /dosyaYoulu2`

---

### Change permissions of files**

> `chmod -R 777 [filename]`

---

### Copy

> `cp -rv {from} {to}`

**r option** = recursive (klasör kopyalamada kullanılır)

**v option** = ilerlemeyi ekrana bastırır

---

### Sistemde Yüklü olan paketleri listeleme

> `dpkg -l`

---

### İki işlemi birleştirme " | " operatörü ile yapılır

> `ls -la | grep workspace`

---

### Ağ Bilgisi Alma

> `netcfg`

---

### Biçimlendirme

> önce `umount` yapılır ardından `mkfs` yapılır

---

### Echo kullanımı

> `echo String >> PATH` = dosyanın sonuna yazar

> `echo String > PATH` = dosyanın üstüne yazar

---

### Path Ekleme

`.bashrc` veya `.zprofile` gizli klasörüne path tanımlanır.

> `nano .bashrc`

> export PATH=~/FoldersPath:$PATH <br>
> export ANDROID_HOME=~/Library/Android/sdk%

bu dosyayı değiştirdikten yapılan değişiklikleri aktifleştirmek için alttaki komut çalıştırılır.

`source .bashrc` => güncellemeleri yapar

---

### Sıkıştırma ve Açma İşlemleri

x = xpand / c = compress

> `tar xfvz FILE.tar.gz`

> `tar xfvj FILE.tar.bz2`

> `tar cfvz FILE.tar.gz(sıkıştırılacak dosya yolu ve adı) FILE[](sıkıştırılacak dosyalar) FOLDER[](sıkıştırılacak klasörler)`

> `tar cfvj FILE.tar.bz2 FILE[] /* daha çok sıkıştırır ama işlem uzun sürer`

---

### İnternet Paylaşımı

> `echo 1 > /proc/sys/net/ipv4/ip_forward`

> `iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE`

---

### Ağdaki Cihazları ve Portları Tarama

> `nmap -n 192.168.1.* -p 5555` <br> (ip aralığı verilir, -p port demektir -web erişimi olan cihazlarda genelde 80 portu açıktır-)

> `nmap -n 192.168.1.* -p 5555 | grep open -B 4`

---

### Ağ Bağlantısı Gateway Atama

> `route -n` (şu anki ayarlar görülür)

> `route del default gw 192.168.1.1` (default değer silinir)

> `route add default gw 192.168.1.138` (yeni değer atanır)

---

### DNS Ayarları Değiştirme

> `nano /etc/resolv.conf`

---

### & işareti

Komutun sonuna koyulan & işareti komutu her halükarda sonraki satıra geçirir, komut çalışmaya devam etse bile.

---

### PS AX Komutu

Çalışan süreçleri gösterir

---

### FG Komutu

Durdurulmuş süreçleri ekrana geri getirir

---

### Hostname Değiştirme

> `nano /etc/hostname`

---

### Çalıştırılabilir Dosyaların Yollarını Bulma

> `which *(program adı)`

Örnek kullanım:
> `which java`

---

### Alternatif Kopyalama İşlemi

> `cat (dosya_yolu).(dosya) > (hedef_yol).(dosya)`

---

### Android Cihaz Bilgileri Görme

Usb ile bağlı bulunan cihazın komut satırına girilir ve orada `getprop` komutu çalıştırılır.

> `adb shell`

ardından
> `getprop`

---

### Dosyaları parçalara bölmek için kullanacağımız komut

Mesela bir 5480 kb lık mp3 dosyamız olsun (test.mp3).Bu dosyamızı 1 megabyte’lık parçalara bölelim.

Örnek :
> `split -b1m test.mp3 test.mp3`

> **b** : byte <br>
> **1** : dosyamız 1 megabyte lık dosyalara böler..yada (2-3-4) dosya büyüklüğüne göre. <br>
> **m** : megabyte

İşlem sonunda Dosyalarımız test.mp3as test.mp3ab....şeklinde parçalanacaktır.

Bu dosyayı tekrar birleştirmek içinse --> `cat` komutunu kullanabiliriz

Örnek :
> `cat test.mp3a* > test.mp3`

---

### En son değisen 10 dosyayi listeleme (log dosyalarında kullanılabilir)

> `ls -al --sort=time | head -n 10`

---

### Android cihazlara Wireless üzerinden ADB ile bağlanma yetkisi verme

> `su`
> `setprop service.adb.tcp.port 5555`
> `stop adbd`
> `start adbd`
> `netstat` (açık portları görmek için)

#### Kablo Takılıysa

> `adb tcpip 5555`
> `adb connect 192.168.0.101(device ip):5555(port)`

---

### Android cihazlarda uygulama başlatma

> `am start -a (Intent)android.intent.action.MAIN -n (package_name)/(class_name)`

---

### Android cihazların klasörlerine yazma yetkisi verme

> `mount`
> `mount -o remount,rw /dev/block/mmcblk0p9 /system`

---

### Detaylı Grep Kullanımı

> `grep {aranacak_kelime} -A 30 -B 10`<br> (A: after B: before bulunan satırdan 10 öncesini ve 30 sonrasını gösterir)

---

### Apache Start/Stop

> `sudo /etc/init.d/apache2 stop (start)`

---

### MySQL Veritabanında SQL Çalıştırma

> `mysql -u(root_adı) -p(şifre) (tablo_adı) < (sql'in yolu)`

---

### Kullanımdaki Portları LİSTELEME

> `sudo netstat -tulpn`

---

### CURL ile HTTP İstekte Bulunma

> `curl --data '{"(key)":(value)}' --referer '{value}' http://127.0.0.1:8080/(class_adı)` <br> //post verileri ile adresten istekte bulunma

> `curl -d "key=value&id=1" --referer "www.google.com" http://127.0.0.1:8080/ServletName/ClassName`

> `curl -vlkL https://127.0.0.1:8443 --ciphers DHE-RSA-AES256-SHA` <br> //sertifika olmadığı halde; ssl bağlantı üzerinden istekte bulunma

> `curl -i -X {GET|POST|PUT|DELETE} -H'Content-Type: application/json' -H'Accept: application/json' -d '{"i_app": "12"}' http://127.0.0.1:8080` <br>
// -X ile istek tipini belirliyoruz. -H ile headerları belirliyoruz, Content-Type : Gönderilecek parametrelerin tipi, Accept : Geri Dönecek Veri Tipi

---

### SSH ile Dosya Kopyalama

> `scp {kopyalanacak_dosya_yolu} {uzak_sunucu_kullanıcı_adı}@{uzak_sunucu_ip}:{uzak_sunucu_kopyalama_dosya_yolu}`

Klasör kopyalamak için -r parametresi kullanılır.

Port belirtmek için -P parametresi kullanılır.

---

### Dosya İçeriği Okuma

> `cat out07.log | grep com.google.android.apps.plus | awk -F\| '{ print $2 }' | sort | uniq | wc -l`

> dosyayı ekrana yazdır || kelimeyi satırlarda ara || -F den sonra ayıraç tanımlanır print &2 ise 2.sütun demek || sırala || arka arkaya aynı olanları çıkar || satır sayısı

> `head -{satır_sayısı} {dosya_yolu}` <br> // dosyanın ilk n satırını okur

> `tail -{satır_sayısı} {dosya_yolu}` <br> // dosyanın son n satırını okur

> `tail -F {dosya_yolu}` <br> // dosyayı güncel takip eder

> `awk -F\| '{ if($2==23) print $0"\n\n"}' {dosya_yolu}` <br> // 2. sütun 23 olan bütün satırları bastır

> `wc -l {dosya_yolu}` <br> // dosyanın satır sayısını gösterir

> `grep -w "pattern" -c {dosya_yolu}` <br> // dosyadaki belli parametreyi içeren satır sayısını gösterir. -c => satır sayısı, -w => girilecek kelime

> `grep -i ali` <br> // büyük küçük harf farketmez (insensitive)

> `grep 'ali\|veli'` <br> // ali veya veli geçen satırları arar

---

### Komutun Durmasını Engelleme

> `nohup {komut_adı}`

---

### MYSQL KOMUTLARI

Veritabanı yedeği alma
> `mysqldump -u{user_name} -p {veritabanı_adı} > ./yedek.sql`

Veritabanı geri yükleme
> `mysql -u{user_name} -p {veritabanı_adı} < ./yedek.sql`

Mysql giriş
> `mysql -u{user_name} -p{password}`

Bağlantıları Görme
> `show processlist`

Bağlantıları Görme Detaylı
> `SELECT * FROM INFORMATION_SCHEMA.PROCESSLIST`

Mysql değişkenlerini listeleme
> `show variables`

Spesifik bir değişkeni görme
> `show variables like "max_connections";`

Değişken atama
> `set global max_connections = 200;`

Bu değişiklik mysql yeniden açıldığında unutulur. Kalıcı değişiklik için /etc/mysqld/my.cnf dosyasının içindeki max_connections satırı değiştirilmelidir

Database seçme
> `use {veritabanı_adı}`

Tablo yapısını görme
> `describe {tablo_adı}`

Mysqldeki tüm databaseleri görme
> `show databases`

Veritabanındaki tüm tabloları görme
> `show tables`

Veritabanı silme
> `drop databse {veritabanı_adı}`

Yeni veritabanı oluşturma (utf-8 li)
> `CREATE DATABASE {veritabanı_adı} DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci`

Veritabanında trigger oluşturma
> `CREATE TRIGGER {trigger_adı} AFTER INSERT ON {etkiyi_başlatan_tablo_adı} FOR EACH ROW INSERT INTO {etkilenecek_tablo_adı}(`id`,`type`) VALUES(NEW.i_episode, 2);`

---

### Uzak Sunucudaki Dosyayı Mount Etme

> `sshfs {user_name}@{ip}:/{uzak_dosya_yolu} {local_dosya_dizini}`

#### Unmount Etme

> `df -h` <br>
> `sudo unmount -f {dosya_yolu}`

---

### APACHE TOMCAT

START TOMCAT
> `cd $CATALINA_TOMCAT_HOME/bin` <br>
> `./startup.sh`

SHUTDOWN TOMCAT
> `cd $CATALINA_TOMCAT_HOME/bin` <br>
> `./shutdown.sh`

Webapps klasörünün altındaki *.war* uzantılı dosyalar çalışmaya başlar, isimlerine göre istek yapılabilir.

Request example :
> `curl -d "{parameters ex :'func=getConf'}" {server_ip}:{port_no: default 8080 or 80}/{project_or_war_file_name}/{class_name}`

---

### RUN JAVA PROGRAM

> `java {min_memory} {max_memory} -classpath {jar_file} :{nesessary_libraries_folder} {Main_Class_Name} 2>>{Error_Log_File} >> {Log_File} {parameter}`

Örnek kullanımlar:
> `java -Xms1G -Xmx12G -classpath StatsMob_Parser_out07.jar:./lib/* com.argeus.packagelistparser.Main 2>>StatsMob_Parser_out07.err >> StatsMob_Parser_out07.out param1`

> `java -jar DBChanger.jar 2>>hata.txt >> normal.txt param1 param2 param3`

---

### CLEAR COMMAND LINE HISTORY

> `history -c` => Delete all history

> `history -d (line of code ex: 5)` => Delete command at specific line

> `history -w` => rewrite on to old data

---

### MERGE PDF FILES IN ONE

> `pdfunite in-1.pdf in-2.pdf in-n.pdf out.pdf`

---

### ANDROID GET PACKAGE LIST OF DEVICE

`adb shell` ile cihazın komut satırına düşülür ardından ilgili komut çalıştırılır.

> `pm list packages`

---

### ANDROID GET FINGERPRINT OF DEBUG KEY

> `keytool -list -v -keystore {path_of_keystore}`

Örnek kullanım:
> `keytool -list -v -keystore ~/.android/debug.keystore`

> `keytool -list -v -keystore ~/.android/debug.keystore -alias {alias_name}`

Örnek kullanım:
> `keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey`

---

### BUILD JAVA PROJECT WITH MAVEN

> `mvn package`

---

### ANDROID PRESENT UNKOWN DEVICE TO ADB

> `nano /etc/udev/rules.d/51-android.rules` <br>
> add new vendor to file (SUBSYSTEM=="usb", ATTR{idVendor}=="0bb4", MODE="0666", GROUP="plugdev")

---

### SOCKS PROXY TANIMLAMA - SSH ÜZERİNDEN

> `ssh -D 8080(port) {server_ip_adresi}` <br>
> ardından firefoxta preferences->advanced->network->settings tabında manuel proxy SOCKS host 127.0.0.1 port 8080(port) atanır <br>
> port çakışması olması durumunda farklı port denenmeli

---

### LINUX SİSTEMDE DOSYA ARAMA

> `find {aranılacak_dizin} -name {aranılacak_dosya_adı - regular exp kabul ediyor -}`

Ornek Kullanım:
> `find /samanlik/ -name "topluigne.txt"`

---

### JAVA UYGULAMALARININ BİLGİLERİNİ ÖĞRENME

> `jinfo {process_id}`

---

### AÇIK PORTLARI LİSTELEME

> `sudo netstat -tulpn`

---

**< [Go to homepage](/)**
