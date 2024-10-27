# Proje Kurulumu ve Yapılandırma
NOT= geolocation, aeap.conf, pjsp.conf, extension.conf, ari.conf, phoneprov.conf, cdr.conf dosyalarında kendi sisteminize göre düzenlemeler yapmanız gerekmektedir.
Ayrıca, IAX protokolü ile de sorunsuz çalışmaktadır. iax.conf dosyasını sisteminize göre düzenleyiniz.

## 2. 21.x ve Üstü Versiyonlar
### 2.1. Kurulum Adımları
21.x sonrası versiyonlar için aşağıdaki değişiklikleri uygulamanız gerekmektedir:


1. 21.x klasöründeki dosyaları kullanınız.
2. İlk kurulum sırasında Docker Compose'u --build tag'i ile başlatmanız gerekmektedir:

```bash
docker-compose up -d --build
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
```
