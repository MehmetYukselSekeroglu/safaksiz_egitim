# Sub Domain keşfi:

<p>
Subdomain keşfine başlamadan önce domain nedir nasıl çalışır mantığını anlamamız gerekmektedir. Bilinen adı ile DNS (Domain Name System) bu yapılar hiyearşik bir isimlendirme yapısına sahiptir. Bunlar linux dosya sistemi gibi ağaç yapıdadırler domainler kendi aralarında alt grupları bulunmaktadır ve bu alt gruplarında grupları mevcuttur ama netice olarak hepsi ROOT "kök" bir domaine bağlıdır.

                        ROOT
          _______________|________________
          |       |            |         |              
       .com      .gov         .edu      .org    ----->  Top Level Domain (TLD)
          |                              |
       google                         wikipedi  ----->  2. seviye Domain
          |                              |
        docs (docs.google.com)           tr     ----->  Alt domain (Subdomain)


<br>
<br>
<h2>FQDN (Fully Qualified Domain Name):</h2>

DNS ağacında hiyerarşisinde bir bilgisayarın tam adresini belirten tamamlanmış addır örnek: mail.google.com bir FQDN dir.

NOT: domainler sadece dış ağda kullanılmazlar iç ağdada kullanılabilirler en basit örnek bilgisayar adınızı google a yazınca kendi sisteminizin bunu tanımasıdır.
<br>
<br>

<h2>Neden domain kullanırız?:</h2>

Domainler bilgisayarlar için değildir çünkü bilgisayarlar netice olarak konusma dilini anlayamazlar ve bunları adreslere çevirirler bu nedenle domainler insanların adresleri hatırlayabilmesi içindir mesela 243.23.8.66 adresi gibi onlarca adresi hatırlamakmı daha kolay yoksa `google.com`, `yandex.com` gibi adreslerimi zaten cevap belli.
<br>
<br>

<h2>DNS server nedir ne için vardır?:</h2>
Deminde dediğimiz gibi bilgisayarlar domainlerden anlamaz ve bunları ip adreslerine çevirmek isterler bu nedenle dns serverleri kullanırlar dns serverler asıl olarak bu isimlerin ip karşılıklarını tutan sunuculardır bu sayede çözümleme yapılabilmektedir mesela 2000 li yılların başında dns serverlara toplu bir ddos saldırısı ile dünya çapında pek çok web sitesine erişim engellenmişti buna neden olan web sitelerinin çökmesi değil bilgisayarların ilgili sitelere ait ip adreslerini bulamamalarıdır. Dns serverler sadece isimden ip değil ip den isim çözümlemeside yapabilmekteler ve bu iki tip kayıtları arada kendi aralarında eşitlerler bunada `zone transferi` denmektedir bu konudaki zafiyetler diğer dersin konusudur ama.

<br>
<br>

<h2>Herkes her domaini alabilirmi?:</h2>
Belirli kurallar dışında evet herkes her domaini alabilir ama bazı domainleri sadece belirli kurumlar alabilmektedir.


| Domain | Anlamı veHerkes alabilirmi? |
|--------|-----------------------------|
| .edu   |  Eğitim kelimesinin ingilizce karşılığıdır yüksek öğretim kurumları (üniversiteler) içindir herkes alamaz. |
| .k12   |  Gene eğitim kurumları içindir ama bu sefer lise ve diğer okullar içindir herkes alamaz bunuda. |
| .gov   |  Goverment yani devlet kelimesinden gelmektedir yanlızca devletlerin alabildiği bir domaindir. |
| .bel   |  Türkiyedeki belediyeler içindir başkası alamaz. |
| .mil   |  Military yani askeri domaindir ordu kurumları içindir bunuda herkes alamaz. |
| .com   |  Standart ticari domaindir herkes alabilir. |
| .net   |  Gene standart ticari domain herkes alabilir. |
| .org   |  Organization dan gelmektedir genelde dernekler ve diğer organizasyonlar kullanır ama herkes alabilir.|
| .xyz   |  Genelde ucuz yollu site kuracaklar veya phishing yapacaklar kullanır |
| .tr .fr|  Bölgesel domainleri temsil eder herkes alabilir.|

<br>
<br>


<h2>Subdomain nedir ve neden keşfini yapıyoruz:</h2>
Önemli olan noktaya geldik subdomain kelime olarak alt alan adıdır ve aslında buna uygundur firmalar site sahipleri vs 
ek hizmetlerini, yeni web projelerini veya diğer hizmetlerini yayınlamak için kendi web adreslerine ait alt adresler 
açarlar mesela buna örnek olarak `forum.kali.org` örnek verebiliriz. Neden keşfini yapıyoruz kısmına gelirsek güvenlik testleri sırasında işimize yaramalarından kaynaklıdır mesela yeni bir forum yazılımı vs test ederken bu domaini bularak bilgi toplanabilir, açık unutulmuş ana domaine bağlı servisler vs olabilir bu nedenle.
<br>
<br>


<h2>Nasıl keşfedilir ve mantığı nedir?:</h2>
Mantığı basittir web dizin keşfindeki gibi sözlük saldırısı veya kaba kuvvet denebilir bir liste ile domaine sorgular atıyoruz şu domain varmı diye cevap olumlu ise subdomaini tespit etmiş oluyoruz ama zamandan kazanmak için bunları araçlar ile yapacaz. Bunların yanı sıra VirusTotal vs gibi servislerdende bulabiliriz ama bu herzaman mümkün olamayabilir.

<br>



<h2>subfinder sub domain keşfi:</h2>
Subfinder aracı golang ile yazılmış basit ve kullanışlı bir keşif aracıdır hemen kuruluma baslayalım.

<br>

```bash
# debian based linux dağıtımları için
sudo apt install -y subfinder 
```

<br>
<h3>Parametreleri ve anlamları: </h3>

| parametre | açıklaması | örnek |
|-----------|------------|-------|
| -d --domain | hedef domaini vermemizi sağlar | -d google.com | 
| -o --output | sonuçları isteğe bağlı olarak dosyaya yazdırır | --output kayıtlar.txt |
| --slient | sadece sub domainleri gösterir diğer çıktıları gizler | --slient (parametre almaz) |
| --config | özel yapılandırma dosyası belirtmeyi sağlar | --config myconf.yaml |
| --source | arama için kullanılacak kaynakları özel olarak belirtmemizi sağlar | --source virustotal, passivetotal |
| --recursive | her sub domaini tekrar ana domain gibi tarar `öz yineleme` | --recursive (parametre almaz) |
| --no-passive  | pasif tarama `osint` kaynaklarını devre dışı bırakır | --no-passive  (parametre almaz) |
| --exclude-sources | belirli bir tarama kaynağını kullanmamasını sağlar | --exclude-source virustotal | 
| --timeout | istekler için zaman aşımı süresini belirtir ön tanımlı 10sn | --timeout 20 |
| --retries | istek başarısız olursa tekrar deneme adetini belirtir ön tanımlı 1 dir | --retries 4 |
| --max-time | taramanın en fazla nekadar süreceğini belirtmeyi sağlar sn cinsindendir ön tanımlı değeri 300 | --max-time 500 |
| --no-resolve  | dns çözümlemeyi devre dışı bırakır | --no-resolve (parametre almaz) | 
| --resolver | çözümleme için özel adres belirtmemizi sağlar | --resolver 1.1.1.1,8.8.8.8 |
| --set-resolvers | önceden tanımlanmıs dns çözümleme hizmeti belirtmemizi sağlar | --set-resolvers cloudflare |
| --verify | SSL sertifikasını doğrulamayı etkinleştirir | --verify (parametre almaz) |
| --insecure | SSL sertifikasını doğrulamayı devre dışı bırakır | --insecure (parametre almaz) | 
| --show-dns | dns kayıtlarını gösterir çıktıda | --show-dns (parametre almaz) | 
| --show-http | http başlıklarını görüntüler | --show-http  (parametre almaz) |
| --show-body | http isteklerinin gövdelerinide gösterir |  --show-body (parametre almaz) |
| --show-all | hepsini gösterir |  --show-body (parametre almaz) |



</p>