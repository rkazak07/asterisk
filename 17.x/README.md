# Proje Kurulumu ve Yapılandırma

NOT= Bu proje, **16.x**, **17.x** ve **21.x** sonrası versiyonlar için yapılandırma ve kurulum adımlarını içermektedir. Ayrıca, *IAX protokolü* ile de sorunsuz çalışmaktadır. Aşağıdaki adımları takip ederek sistemi kurabilirsiniz.

## 1. 16.x ve 17.x Versiyonları

### Kurulum Adımları

1. **17.x** klasörü içerisindeki dosyaları kullanınız.
2. İlk kurulum sırasında Docker Compose'u `--build` tag'i ile başlatmanız gerekmektedir:

   ```bash
   docker-compose up -d --build

Windows için

   ```bash
   docker-compose -f docker-compose-windows.yaml up -d --build
```


## 1.2. Yapılandırma Dosyaları
Projede tüm yapılandırma dosyaları configs klasörü içerisindedir. İç hat numaralarını tanımlamak için sip.conf ve extensions.conf dosyalarını düzenlemeniz yeterlidir.


### 1.2.1. sip.conf Örneği
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

### 1.2.2. extensions.conf Örneği
SIP numarasının dahili çağrılarda nasıl çalışacağını tanımlamak için:

```
[internal]
exten => 2222,1,Dial(SIP/2222)
```

### 1.3. Değişikliklerden Sonra Yeniden Başlatma
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
