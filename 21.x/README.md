# Proje Kurulumu ve Yapılandırma
NOT= geolocation, aeap.conf, pjsp.conf, extension.conf, ari.conf, phoneprov.conf, cdr.conf, rtp.conf dosyalarında kendi sisteminize ve ihtiyaçlara göre düzenlemeler yapmanız gerekmektedir.
Ayrıca, IAX protokolü ile de sorunsuz çalışmaktadır. iax.conf dosyasını sisteminize göre düzenleyiniz.

## 2. 21.x ve Üstü Versiyonlar
### 2.1. Kurulum Adımları
21.x sonrası versiyonlar için aşağıdaki değişiklikleri uygulamanız gerekmektedir:


1. 21.x klasöründeki dosyaları kullanınız.
2. İlk kurulum sırasında Docker Compose'u --build tag'i ile başlatmanız gerekmektedir:

```bash
docker-compose up -d --build
```

Windows için

```bash
docker-compose -f docker-compose-windows.yaml up -d --build
```


## 2.2. Yapılandırma Dosyaları
Projede tüm yapılandırma dosyaları configs klasörü içerisindedir. İç hat numaralarını tanımlamak için pjsip.conf ve extensions.conf dosyalarını düzenlemeniz yeterlidir.

### 2.2.1. pjsip.conf Örneği
Aşağıda bir PJSIP numarası ekleme örneği verilmiştir:

```
[6001]
type=endpoint
context=internal
disallow=all
allow=ulaw
auth=auth6001
aors=6001

[auth6001]
type=auth
auth_type=userpass
password=6001
username=6001

[6001]
type=aor
max_contacts=1
```

### 2.2.2. extensions.conf Örneği
SIP numarasının dahili çağrılarda nasıl çalışacağını tanımlamak için:

```
[internal]
exten => 6001,1,Dial(PJSIP/6001)
```

### 2.3. Değişikliklerden Sonra Yeniden Başlatma
Yapılandırma dosyalarında değişiklik yaptıktan sonra Docker Compose’u yeniden başlatmanız gerekmektedir. Aşağıdaki komutlarla önce kapatıp sonra tekrar başlatabilirsiniz:

```bash
docker-compose down
docker-compose up -d
```
Windows için

```bash
docker-compose down
docker-compose -f docker-compose-windows.yaml up -d
```
### 2.3.1 Linux işletim sistemleri için pjsip.conf dosyasındaki [transport-udp] alanında host ip adresinin eklenmesi gerekmektedir. Aksi takdirde ses iletilmeyecektir

[transport-udp]
type = transport
protocol = udp
bind = 0.0.0.0
local_net=host-ip-adresi/32
external_media_address=host-ip-adresi
external_signaling_address=host-ip-adresi
external_signaling_port=5060
