

### Wordpress Web Siteleri Temel Güvenlik Önlemleri

1. **Sürümler:**
    * Deprecated sürümler kullanılmamalı
    * Wordpress son sürümünde mi?
    * PHP Güncel sürümünde mi?
    * Tüm pluginler son sürümünde mi?
    * Veritabanı altyapısı güncel sürümünde mi?
2. **Domain**
    * Domain SSL sertifikası var mı?
    * Non SSL versiton SSL'e yönleniyor mu? http >> https
    * www versiyon yönleniyor mu? 

3. **Dosyalar ve Dizin** **Yetkileri**
    * Klasörler 755
    * Dosyalar 644	
    * .htaccess , wp-config.php 640
    * Dosya sahibi : www-data veya non-root user
    * **Linux Bash Script:**



    ~~~
       chown www-data:www-data  -R * # Let Apache be owner
       find . -type d -exec chmod 755 {} \;  # Change directory permissions rwxr-xr-x
       find . -type f -exec chmod 644 {} \;  # Change file permissions rw-r--r–
    ~~~


![filemods](assets/filemods.png "File mods and owners")



4. **Genel Önlemler**
    * wp-admin/ giriş adresinin değiştirilmesi ( panel, yonetim, vb )
    * wp-admin/ login recaptcha robot değilim onay kutulu v2 
    * Tahmin edilebilir , ( Admin veya domain adında )  kullanıcısının bulunmaması
    * Commentler kapalı olmalı.
    * Silinebilecek **gereksiz** WP dosyaları ( güvenlik açığı oluşturabilir )
        * /licence.txt
        * /readme.html
        * /wp-trackback.php
        * /xmlrpc.php
        * /wp-comments-sample.php
    * Remove wp version from source

5. **Development dosyalarının silinmesi**
    *  **Site root dizinde:**
        * test.php, info.php vb özellikle phpinfo gösteren dosya ve benzerleri **production **da  silinmeli.
        * **FTP programları** config dökümanları ( .ftpconfig .sitesettnigs .vb ) Kök dizinde olmamalı.
        * Javascript kaynak dosyaları .min versiyonu kullanılmalı.
        * Console.log kapalı olmalı.
        * wp-config.php 
        * SALT key ler güncellenmeli. [SALT Generator](https://api.wordpress.org/secret-key/1.1/salt/)
        * Loglama aktif ve görünmez olmalı, log dizinleri publicê kapalı olmalı.
        * **wp-config.php ye ekle**


    ```
        ini_set('display_startup_errors', 0);
        ini_set('display_errors', 0);
        $today =  date('y-m-d');
        error_reporting(E_ALL);
        define( 'WP_DEBUG', true );
        define( 'WP_DEBUG_DISPLAY', false  );
        define( 'WP_DEBUG_LOG', 'logs/debug-'.$today.'.log' );
        // do not save post revisions
        define('WP_POST_REVISIONS', false);
        // Do not update WP core automaticaly
        define( 'WP_AUTO_UPDATE_CORE', false );
        // Disable file modification and plugin install from panel
        define('DISALLOW_FILE_MODS', true);
        define('DISALLOW_FILE_EDIT', true);
        // define('WP_DISABLE_FATAL_ERROR_HANDLER',true);
    ```
    * Tema veya plugin dosyalarının Wordpress dışında çalıştırılmasını engelle.
    Bu scripti dosyaların başına ekleyebilirsin.
        *  ~~~~
                <?php defined('ABSPATH') or die(); ?>
        


6. **Database**
    * DB Prefix wp_ den farklı olmalı
    * **DBUSER** **root olmamalı**.
    * Sadece bu sitenin DB sine erişebilecek bir kullanıcı olmalı. 

7. **Pluginler**
    * İnaktif pluginler silinmeli
    * Tüm pluginler son sürümünde olmalı
    * Wordpress.org tan indirilmiş veya lisans ile geliştirici sitesinden indirilmiş olmalı.
    * **Tavsiye edilen pluginler**
        * [All In One WP Security](https://wordpress.org/plugins/all-in-one-wp-security-and-firewall/)
        * [Disable REST API](https://wordpress.org/plugins/disable-json-api/)
        * [Safe SVG](https://wordpress.org/plugins/safe-svg/)
        * [SameSite](https://wordpress.org/plugins/samesite/)
        * [Clean Image Filenames](https://wordpress.org/plugins/clean-image-filenames/)
        * [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/)
        * Login No Captcha reCAPTCHA
        * Migration 
            * [All-in-One WP Migration](https://wordpress.org/plugins/all-in-one-wp-migration/) + All-in-One WP Migration Unlimited Extension
            * [Better Search Replace](https://wordpress.org/plugins/better-search-replace/)
            * [WP File Manager](https://wordpress.org/plugins/wp-file-manager/)





8. **Ideal Deployment Cycle:**
    * Wordpress ve Pluginler panel üzerinden güncellenir
    * **Aktif theme development** yapılıyor ise ; 
        * Geliştirmesi yapılan temalar bu kategoride. 
        * Sadece **./wp-content/themes/tema_adi/ **içerisinde güncelleme olur.**
        * **Local development** ortamı kurulur, geliştirmeler burada yapılır
        * **Staging domain** var ise, lokal geliştirmeler buraya remote FTP veya github üzerinden push edilir
        * Staging domain, **Production’a** aktarılır
        * Sadece Production Domain ise;
            * Dosya ve Veritabanı Yedekleri alınır.
            * Localde test edilen scriptler Production ‘a remote FTP veya github aracıyla yüklenir.
    * **Aktif development yok ise:**
        * Satın alınmış temalar bu kategoride. 
        * Sadece production domain ve panel üzerinden güncellemeler yapılır.
