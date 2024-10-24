
# Proje Kurulumu ve Yapılandırma

Bu ürün 16.x ve 17.x versiyonları için geliştirilmeye başlanmıştır. Güncel versiyonlar için çalışmalar devam etmektedir. Ayrıca *IAX protokolü* ile de sorunsuz çalışmaktadır.

## Kurulum Adımları

İlk kurulum sırasında Docker Compose'u *--build* tag'i ile başlatmanız gerekmektedir:

```bash
$ docker-compose up -d --build
```

Yapılandırma Dosyaları

Projede tüm yapılandırma dosyaları configs klasörü içerisindedir. İç hat numaralarını tanımlamak için sip.conf ve extensions.conf dosyalarını düzenlemeniz yeterlidir.

sip.conf Örneği

Aşağıda bir SIP numarası ekleme örneği verilmiştir:
```
[2222]
type=friend
secret=1234
host=dynamic
context=internal
nat=no
canreinvite=no
disallow=all
allow=ulaw
allow=alaw
allow=gsm
dtmfmode=rfc2833
callerid="2222"
```
extensions.conf Örneği

SIP numarasının dahili çağrılarda nasıl çalışacağını tanımlamak için:
```
[internal]
exten => 2222,1,Dial(SIP/2222)
``
Değişikliklerden Sonra Yeniden Başlatma

Yapılandırma dosyalarında değişiklik yaptıktan sonra Docker Compose’u yeniden başlatmanız gerekmektedir. Aşağıdaki komutlarla önce kapatıp sonra tekrar başlatabilirsiniz:

```bash
$ docker-compose down ; docker-compose up -d
 ```
