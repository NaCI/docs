# Android Network Security

Android uygulamarda network bağlantımızı güvenli ve gizli tutmak için tüm istekler HTTPS/TLS kanal üzerinden yapılmalı.

Fakat cihazda zararlı yazılımlarla veya farklı yöntemlerle HTTPS trafiğinin arasına girilebiliyor. Bu işlem için cihaza zararlı bir sertifika
Certificate Authority (CA) gibi tanımlanıp network trafiği dinlenip arasına girilebiliyor. Yazının devamında bunun nasıl engellenebileceğine dair notlar var.

## Araştırma Notları

- Android sertifika değişimlerinde önceki versiyonlara destek verme sorunlarından dolayı **SSL Pinning’i tavsiye etmiyor**. Yapılacaksa da mutlaka bir backup pin değeri de olması gerektiği belirtiliyor.

- SSL Pinning yerine, cihazlar sadece sistemde tanımlı sertifika otoritelerine güvenmeye zorlanabilir. Bu durumda araya girmek isteyen zararlı bir yazılım cihaza kendi sertifika otoritesini koysa da bağlantıyı dinleyemiyor. Fakat bu destek android 7 (api 24) ve üzerindeki cihazlar için geçerli. Ayrıca emulator’lere user dışında system sertifikası da kurulabiliyor. Emulatorler için bu güvenlik bypass edilebiliyor.

- Bunu da engelleme için network-security-config altına sadece kendi belirlediğimiz CA’lerin tanımını yapıp uygulamanın sadece bu CA’lere güvenmesini sağlayabiliyoruz. Burada piyasadaki güvenilir birkaç CA bu listeye eklenir ve gelecekteki sertifikalar bu CA’lerden alınırsa sorun çözülmüş oluyor. Yeni bir CA gelmediği sürece force push gerekmiyor. (Benzer çözüm sslpinningde leaf’e değil root’a güvenerek de yapılabilir ama orda tek CA koymuş oluyoruz)

> NOT: network-security-config ile güvenlik desteği android 7 (api 24) ve üzerindeki cihazlar için var.

## Uygulama örneği

- Bunun için de CA’in cer dosyasını indirip uygulamada res->raw altına atıyoruz. Ardından xml->network_security_config dosyamızda şöyle bir config tanımı yapıyoruz:

```xml
<network-security-config>
    <base-config cleartextTrafficPermitted="false">
        <trust-anchors>
            <certificates src="@raw/usertrust_ca" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

> Not: Burada base-config yerine domain-config ile sadece kendi endpointimizi tanımlamamız daha sağlıklı, bu sadece uygulamadaki farklı endpointleri (görseller için gidilen cdn’ler vs.) zorlamamış oluruz.

Son olarak da AndroidManifest dosyasında application tag'i içine `android:networkSecurityConfig="@xml/network_security_configuration"` tanımı yapıyoruz.

**REFERENCES**
: [https://developer.android.com/training/articles/security-config.html](https://developer.android.com/training/articles/security-config.html)
: [https://developer.android.com/codelabs/android-network-security-config](https://developer.android.com/codelabs/android-network-security-config)

**< [Go back to Android section](../android)**
