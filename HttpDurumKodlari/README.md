# HTTP Durum Kodları
<br>

## HTTP Durum kodları nedir ve ne için vardır?

<p>
    HTTP durum kodları istemci ve sunucu arasındaki iletişimi
    standartlaştırmak ve iletişim durumunun anlaşılması içindir
    temel olarak 5 çeşitlerdir ve 3 haneli kodlardır.
</p>
<br>

## Durum kodlarının bizim için önemi.

<p>
    Web uygulamalarını api'leri vs test ederken, test etmek için otomatik
    araçlar vs kullanırken sunucunun bize vermiş olduğu tepkiler önemlidir
    mesela dizin taraması işlemlerinde, web kazıma işlemleri gibi pek çok
    noktada önemlidir bunlar.
</p>
<br>
<br>

## Durum kodlarının genel yapısı.
<br>

| Kod Aralığı | Kod aralığının Amacı | 
|-------------|----------------------|
| 1xx         | Bilgilendirme amaçlıdırlar işlem yapmaya gerek yoktur. |
| 2xx         | İşlemin başarılı olduğunu belirken kodlardır. |
| 3xx         | Yönlendirmeyi belirtir başka bir url'e vs. |
| 4xx         | İstemci kaynaklı hataları belirtir. |
| 5xx         | Sunucu kaynaklı hataları belirtir.

<br>
<br>

## Kesinlikle bilinmesi gereken durum kodları:
<br>

| Durum Kodu | Durum Kodu'nun Anlamı |
|------------|-----------------------|
| 200        | İşlem başarılı devam edildiğini belirtir. |
| 301        | İstek yapılan url taşınmış ve yeni adrese yönlendiriliyor. |
| 302        | İstek yapılan url geçici olarak taşınmış yeni adrese yönlendiriliyor.| 
| 400        | BadRequest sunucuya yanlış bir şeklide istek atılmış veya sunucu isteği anlayamamış. |
| 401        | UnAuthorized, Erişilmek istenen url de kimlik doğrulaması gerekli ve istemcinin yetkilendirmesi başarısız veya yapılmamış. |
| 403        | Forbidden, erişim izni olmayan bir konuma erişmek istenmiş demektir. |
| 404        | NotFound, istenilen adres bulunamamış. |
| 500        | Sunucu içerisinde hata olduğu zaman olur. |
| 503        | Sunucu hizmet vermediği veya aşırı yük altında olunca verilir. |












