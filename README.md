  Plesk .env Güvenliği - Hızlı Rehber

📎 Plesk .env Güvenliği - Hızlı Rehber
======================================

Bu rehber, Plesk üzerinde çalışan web projelerinizde .env dosyasının güvenliğini sağlamak için almanız gereken önlemleri adım adım açıklamaktadır. .env dosyasının dışarıdan erişilebilmesi büyük bir güvenlik açığına yol açar. Bu rehberde, hem **NGINX** hem de **Apache** web sunucuları için uygulanabilir çözüm yollarını bulacaksınız.

🔒 Sorun Tanımı
---------------

*   .env dosyası, önemli konfigürasyon bilgileri (veritabanı şifreleri, API anahtarları) içerir.
*   Eğer .env dosyasına dışarıdan erişim sağlanırsa, bu bilgiler kötü niyetli kişiler tarafından çalınabilir.
*   .env dosyasının **indirilebilir olması**, büyük güvenlik tehditlerine yol açar.

* * *

✅ Çözüm: Nginx İle Erişimi Engelleme
------------------------------------

### 1\. Nginx Konfigürasyonunu Düzenleme

Plesk panel üzerinden Nginx’in ek ayarlarını yaparak, .env dosyasına dışarıdan erişimi engelleyebilirsiniz.

#### Adımlar:

1.  Plesk Paneli’ne giriş yapın.
2.  Web Sitesi > Apache & nginx Ayarları sekmesine gidin.
3.  Ek nginx yönergeleri (Additional nginx directives) bölümüne şu kodu ekleyin:

    location ~* /\.env$ {
        return 404;
    }

4.  Kaydet ve Nginx’i yeniden başlatın.

### 2\. Test Etme

Tarayıcınızda gizli sekme kullanarak erişim testini yapın:

    curl -I https://domain.com/.env

Eğer **HTTP/1.1 404 Not Found** yanıtını alıyorsanız, yapılandırma başarılı olmuştur.

* * *

✅ Çözüm: Apache İle Erişimi Engelleme
-------------------------------------

### 1\. Apache Konfigürasyonunu Yapılandırma

Apache kullanıyorsanız, .env dosyasına gelen istekleri engellemek için .htaccess dosyasını düzenleyebilirsiniz.

#### Adımlar:

1.  Plesk Paneli üzerinden Web Sitesi > Apache & nginx Ayarları sekmesine gidin.
2.  Apache web sunucusu sekmesinde şu direktifi .htaccess dosyasına ekleyin:

    <FilesMatch "^\.env$">
        Order allow,deny
        Deny from all
    </FilesMatch>

3.  Kaydet ve Apache’yi yeniden başlatın.

### 2\. Test Etme

Erişim kontrolünü gizli sekme ile tekrar test edin:

    curl -I https://domain.com/.env

Yanıt olarak **404 Not Found** alıyorsanız, Apache yapılandırması başarıyla uygulanmış demektir.

* * *

⚠️ Dikkat Edilmesi Gerekenler
-----------------------------

*   **Dosya taşıma**: .env dosyasını başka bir dizine taşımak, Strapi'nin o dizinden okuma yapmasını engeller. Ancak, bu çözüm geçici bir çözüm olup, konfigürasyon hatalarına yol açabilir.
*   **Proxy Mode**: Eğer Plesk’te Proxy Mode kapalıysa, doğrudan Nginx devreye girer. Bu durumda Apache ayarları etkili olmaz, Nginx konfigürasyonunu kullanmanız gerekir.
*   **Tarayıcı önbelleği**: Yapılandırma sonrasında, hala eski konfigürasyonu görebiliyorsanız, tarayıcı önbelleğini temizlemeyi deneyin veya gizli sekmede test edin.

* * *

🧪 Test Komutları
-----------------

### 1\. Erişim Kontrolü Testi

    curl -I https://domain.com/.env

### Başarılı Yanıt:

    HTTP/1.1 404 Not Found
