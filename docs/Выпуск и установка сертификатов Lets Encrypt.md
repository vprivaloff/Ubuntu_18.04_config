### Выпуск сертификата

[Инструкция](https://timeweb.com/ru/community/articles/apache-ubuntu-18-04-vypusk-i-ustanovka-ssl-sertifikata-let-s-encrypt)

+ Настройка HTTPS

    `sudo ufw allow 'Apache Full'`
    
    `sudo ufw allow ssh`

    `sudo ufw status`
    
    ```
    Output
    
    Status: active
    
    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere                  
    2203                       ALLOW       Anywhere                  
    80,443/tcp                 ALLOW       Anywhere                  
    Apache                     ALLOW       Anywhere                  
    Apache Full                ALLOW       Anywhere                  
    22/tcp                     ALLOW       Anywhere                  
    OpenSSH (v6)               ALLOW       Anywhere (v6)             
    2203 (v6)                  ALLOW       Anywhere (v6)             
    80,443/tcp (v6)            ALLOW       Anywhere (v6)             
    Apache (v6)                ALLOW       Anywhere (v6)             
    Apache Full (v6)           ALLOW       Anywhere (v6)             
    22/tcp (v6)                ALLOW       Anywhere (v6)
    ```

+ Установка Certbot

    `sudo apt-get install software-properties-common`
    
    `sudo add-apt-repository ppa:certbot/certbot`
    
    `sudo apt install python-certbot-apache`

   ` sudo certbot --apache --email admin@example.ru -d example.ru -d www.example.ru`

    >Добавить для основного домена `АААА` запись в DNS (выбрать из списка `ipv6`)
    >Скопировать и добавить такую же `AAAA` для поддоменов
    >После этого можно выпускать сертификаты для поддоменов

    `sudo certbot --apache --email admin@example.ru -d dev.example.ru -d www.dev.example.ru`

+ Проверка автоматического продления срока действия Certbot

    `sudo certbot renew --dry-run`