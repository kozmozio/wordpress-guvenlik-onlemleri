

### Wordpress Web Siteleri Temel Güvenlik Önlemleri


### <p style="text-align: right">
Tarih : 04.04.2023</p>




1. **Sürümler : **Deprecated sürümler kullanılmamalı
        1. Wordpress son sürümünde mi?
        2. PHP Güncel sürümünde mi?
        3. Tüm pluginler son sürümünde mi?
        4. Veritabanı altyapısı güncel sürümünde mi?
2. **Domain **
        5. Domain SSL sertifikası var mı?
        6. Non SSL versiton SSL'e yönleniyor mu?
        7. www versiyon yönleniyor mu? \

3. **Dosyalar ve Dizin** **Yetkileri**
        8. Klasörler 755
        9. Dosyalar 644	
        10. .htaccess , wp-config.php 640
        11. Dosya sahibi : www-data veya non-root user
        12. **Bash Script : **


    ```
       chown www-data:www-data  -R * # Let Apache be owner
       find . -type d -exec chmod 755 {} \;  # Change directory permissions rwxr-xr-x
       find . -type f -exec chmod 644 {} \;  # Change file permissions rw-r--r–


<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")
    ```


4. **Genel Önlemler **
    13. wp-admin/ giriş adresinin değiştirilmesi ( panel, yonetim, vb )
    14. wp-admin/ login recaptcha robot değilim onay kutulu v2 
    15. Tahmin edilebilir , ( Admin veya domain adında )  kullanıcısının bulunmaması
    16. Commentler kapalı olmalı.
    17. Silinebilecek **gereksiz** WP dosyaları ( güvenlik açığı oluşturabilir )
        * /licence.txt
        * /readme.html
        * /wp-trackback.php
        * /xmlrpc.php
        * /wp-comments-sample.php
    18. Remove wp version from source
5. **Development** dosyalarının silinmesi
    19.  **Site root dizinde : **
        * test.php, info.php vb özellikle phpinfo gösteren dosya ve benzerleri **production **da  silinmeli.
        * **FTP programları** config dökümanları ( .ftpconfig .sitesettnigs .vb ) Kök dizinde olmamalı.
    20. Javascript kaynak dosyaları .min versiyonu kullanılmalı.
    21. Console.log kapalı olmalı.
    22. wp-config.php 
        * SALT key ler güncellenmeli. [https://api.wordpress.org/secret-key/1.1/salt/](https://api.wordpress.org/secret-key/1.1/salt/)
        * Loglama aktif ve görünmez olmalı
        * **wp-config.php ye ekle : **

    ```
          ini_set('display_startup_errors', 1);
           ini_set('display_errors', 1);
           $today =  date('y-m-d');
           error_reporting(E_ALL);
           define( 'WP_DEBUG', true );
          	define( 'WP_DEBUG_DISPLAY', false  );
          	define( 'WP_DEBUG_LOG', 'wp-content/uploads/logs/debug-'.$today.'.log' );
          	// do not save post revisions
          	define('WP_POST_REVISIONS', false);
          	// Do not update WP core automaticaly
          	define( 'WP_AUTO_UPDATE_CORE', false );
          	// Disable file modification and plugin install from panel
          	define('DISALLOW_FILE_MODS', true);
          	define('DISALLOW_FILE_EDIT', true);
          	// define('WP_DISABLE_FATAL_ERROR_HANDLER',true);
    ```


6. **Database**
    23. DB Prefix wp_ den farklı olmalı
    24. **DBUSER** **root olmamalı**. Sadece bu sitenin DB sine erişebilecek bir kullanıcı olmalı. \

7. **Pluginler**
    25. İnaktif pluginler silinmeli
    26. Tüm pluginler son sürümünde olmalı
    27. Wp All In One Security tavsiye ediliyor.
    28. Disable REST API tavsiye ediliyor
    29. Safe SVG tavsiye ediliyor \

8. **Ideal Deployment Cycle:**
    30. Wordpress ve Pluginler panel üzerinden güncellenir
    31. **Aktif theme development** yapılıyor ise ; 
        * Geliştirmesi yapılan temalar bu kategoride. 
        * Sadece **/wp-content/themes/tema_adi/ **içerisinde güncelleme olur.** **
        * **Local development** ortamı kurulur, geliştirmeler burada yapılır
        * **Staging domain** var ise, lokal geliştirmeler buraya remote FTP ile push edilir
        * Staging domain, **Production’a** aktarılır
        * Sadece Production Domain ise;
            * Dosya ve Veritabanı Yedekleri alınır.
            * Localde test edilen scriptler Production ‘a remote FTP aracıyla yüklenir.
    32. **Aktif development yok ise : **
        * Satın alınmış temalar bu kategoride. 
        * Sadece production domain ve panel üzerinden güncellemeler yapılır.
