# SSL üzerinden aktif bilgi toplama 


<h1>SSL nedir?: </h1>

<p> 
SSL (Secure Socket Layer) internet üzerindeki trafiğimizin şifrelenmesini sağlayan bir şifreleme protokolüdür sadece web sitelerinde değil mesajlaşma vs gibi pek çok yazılımda kullanılmaktadır en basit örneği httpS dir.


SSL üzerinden daha güvenli ve daha yeni olan TLS (Transport Layer Security) protokolü geliştirilmiştir ama genel olarak hala SSL terimi her ikisinide kapsıcak şekilde kullanılmaktadır.

</p>

<br>
<h1>SSL sertifikası nedir?:</h1>
<p>
SSL bir protokoldür aslında bizim bilgileri toplicamız kısım SSL sertifikasıdır bu sertifka trafiğin şifrelendiğini gösterir bize trafik asimetrik şifreleme ile şifrelnir 
bu ne demek yani bir adet public key ve bir adet de private key ile şifreleme ve çözme işlemleri yapılır istemci public key ile veriyi şifreler sunucu ise private key ile çözer ve anlar trafiği bu sayede sertifikanın imzalanması vs sağlanmaktadır ve bilgiler elde edilebilir.

</p>


<br>

<h1>SSL sertifikası içerisinde hangi bilgileri barındırır?: </h1>
<p>

Bunlara örnek olarak:

- Sertifikanın geçerlilik tarihi (başlangıç ve bitiş)
- Sağlayıcısı ve imzalayan 
- Geçerli olduğu domainler 

Bu bilgiler sayesinde bilgi toplayabilmekteyiz sürümlere göre zafiyetler vs olabilir ama 
artık en basit içerik yönetim sistemi bile ücretsiz ssl sertifikası sağlıyor ve en son sürümde oluyor.

</p>

<br>
<h1>SSL in önemi: </h1>
<p>Daha doğrusu ssl sertifikasının önemi demeliyiz bu sertifika olmadan trafik `clear text` yani okunabilir metin formatında gönderilerek alınır bu durumdada trafiği içeriden veya dışarıdan dinleyen 3.kişiler özel verilere kolaylıkla erişim sağlayabilmektedir 
bu nedenden dolayı ssl kullanırız ssl trafiğin okunmasının önüne geçer ve değiştirilmesini engeller bu nedenle dikkat edilmelidir.

Bir web sitesinde ssl bulunmasının yanı sıra sertifikanın güvenilir taraflar tarafından imzalanmış olmasıda çok önemlidir aksi halde ssl olmasına rağmen imzalanmamış güvenilmez 
bir sertifika ile gene verilerimiz elden gidebilir bunada dikkat etmek gerekir.

</p>


## Normal Olması Gereken SSL:

<img src="img/NormalSSL.png">

<br>
<br>

## SSL yok:

<img src="img/NoSSL.png">

<br>
<br>

## Geçersiz SSL:


<img src="img/UnTrustedSSL.png">

<br>
<hr>
<br>


<h1> Kullanacağımız araçlar ve kurulumları </h1>
<p>
Dersde `metasploit-framework` içerisindeki hazır bir modül ve `sslyze` araçlarını kullanacağız.


Kurulumları:

Metasploit: `sudo apt install -y metasploit-framework`
<br>
SSLyze    : `sudo apt install -y sslyze`


şeklindedir.


</p>

<br>
<br>


<h1>Tarama kısmı: </h1>
<p>
İlk olarak msfconole ile başlayalım metasploit-framewor'ü ilk kez açacaksanız.

`sudo msfdb init` 
<br>
komutunu çalıştırarak msf konsolun veritabanını oluşturabilirsiniz.

<h3>SIRASIYLA ADIMLARIMI:</h3>


Msf konsolu başlatalım <br>
`msfconsole`

Açılan msfconsole ekranına kullanacağımız modülü yazalım <br>
`use auxiliary/scanner/ssl/ssl_version`

Taranacak adresi belirtelim ip veya domain olabilir <br>
`set rhost google.com`

Bu noktadan sonra direk `run` diyerek taramayı başlatabiliriz ama ek ayarları vs görmek isterseniz `show options` komutunu kullanabilirsiniz.

<h3>Sonuçlar ve analizi: </h3>

Sizdeki sonuçlarda aşşağıdakine benzer olacaktır:
<br><br>
<img src="img/MsfResults.png">
<br>
Sonuçları kısım kısım renklerle ayırdım buradaki kısımlar şu şeklilde 


Kırmızı -> Hedefin SSL/TLS sürümünü belirten ve şifreleme algoritmalarını gösterir.

Yeşil -> Setifika içerisindeki bilgileri verir 

Mavi -> Sonuçların kaydolduğu konumu verir


<br>
Yeşil kısımdaki bilgilere bakaraktan:<br>

- Subject: Sertifikanın ait olduğu domainleri belirtir *.google.com demek google ve tüm alt domainlerinde (subdomain) geçerli demektir.

- Issuer: Sertifikayı kimin imzaladığını gösterir burada `Google Trust Service LLC` tarafından imzalanmış.111

- Signature Alg: imzalama algoritmasıdır burada SHA ve RSA kullanılarak imzalandığını görürüz.

- Public key size: Sertifikanın kullandığı genel anahtarın kaç bit olduğunu gösterir.

- Not valid before: Sertifikanın hangi tarihten sonra geçerli olduğunu gösterir 

- Not valid After: Sertifikanın geçersiz olacağı tarihi verir.

- CA imzalayanı: Sertifikayı imzalayan hakkında gene.

- Has common name: Sertifikanın ana alan adıdır bağlantı doğrulamak için kullanılır.



<hr>
<br>
<br>
Şimdide `sslyze` aracı ile taramamızı yapıp sonuçlarına bakalım:

<h3>SIRASIYLA ADIMLAR:</h2>

Çok fazla spesifik parametreler istemez direk domaini veya ip adresini iteğe bağlı olarakda ssl portunu verebilirsiniz.

`sslyze google.com `


<h3>Sonuçlar ve analizi: </h3>

Gene sonuçlarımız benzer olacaktır:
<br>
<img src="img/sslyze_1.png">
<br>

Renklere göre çıktıyı parçalara ayırdım:

Kırmızı ile gösterilen kısımda domain'in çözümlenmesi ve ip adresinin bulunmasını sağlıyor ve sıradaki kısma geçiyoruz.

Yeşilli kısımda bizi msf konsol çıktısına neredeyse aynı denebilecek bir çıktı karşılıyor değerler vs aynı zaten bu noktayı tekrar anlatmaya gerek yok.

Mavi kısım bu tool'daki en önemli kısım denebilir neden?
SSL içerisinde geçerli olduğu domainleri bulundurur buda otomatik olarak subdomainleri verir yani sublis3r vs den daha hızlı ve daha az iz bırakarak sağlar bunu bu nedenle subdomain keşfinde kullanılması gereken adımlardan biridir.

Şimdi çıktının devamına bakalım:
<br>
<img src="img/sslyze_2.png">
<br>

Bu kısımda ise bize en çok lazım olacak olan nokta ssl ve tls nin sürümlerine bağlı olarak bilinen güvenlik açıklarını vs kapsayıp kapsamadığını gösteriyor bu noktayı yeşil kare ile belirttim. Gösterilen zafiyetler vs bugunun konusu değil ama gözden kaçmaması gereken bir nokta sonuç olarak.

Çıktının diğer kısımlarını merak edenler için ise kutunun üst kısmındaki alanda şifreleme hakkında bilgiler içeriyor alt kısmında ise desteklenen anahtar değişim sistemleri vs veriyor.


SSL/TLS üzerinden bilgi toplama dersi şimdilik bukadardır.
</p>