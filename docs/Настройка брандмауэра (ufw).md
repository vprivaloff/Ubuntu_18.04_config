# Настройка брандмауэра (ufw)

+ Проверить OpenSSH командой

    `ufw app list`
    
    ```
    Output
    
    Available applications:
    OpenSSH
    ```

+ Необходимо убедиться, что брандмауэр разрешает SSH-соединения, чтобы можно было войти в систему в следующий раз. Мы можем разрешить эти соединения путем ввода:
    
    `ufw allow OpenSSH`

+ Активировать OpenSSH

    `ufw allow OpenSSH`
    
+ Активировать брандмауэр 

    `ufw enable`
    
+ Проверить брандмауэр

    `ufw status`

    ```
    Output
    Status: active
    
    To                         Action      From
    --                         ------      ----
    OpenSSH                    ALLOW       Anywhere
    OpenSSH (v6)               ALLOW       Anywhere (v6)
    ```

+ Разрешаем доступ к порту 2203

    `sudo ufw allow 2203`

+ Разрешаем доступ HTTP/HTTPS (port 80,443)

    `sudo ufw allow proto tcp from any to any port 80,443`