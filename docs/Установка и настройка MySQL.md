###  Установка MySQL

+ Обновить индекс пакетов

    `sudo apt update`
    
+ Затем установить пакет

    `sudo apt install mysql-server`
    
###  Настройка MySQL

+ Выполнить скрипт безопасности командой:

    `sudo mysql_secure_installation`
    
    >В результате выполнения этого скрипта вам будет предложено внести изменения в настройки безопасности вашей MySQL. Сначала вам будет предложено установить плагин валидации паролей (Validate Password Plugin), который позволяет тестировать надёжность паролей MySQL. Далее вам предложат задать пароль для пользователя root вашей установки MySQL. Выберите надёжный пароль и введите его два раза.

    >Далее вы можете выбирать Y и нажимать ENTER для всех последующих вопросов. При этом будут удалены некоторые анонимные пользователи и тестовые базы данных, будет отключена возможность удалённого входа для root пользователей, после чего все внесённые изменения будут применены к вашей установке MySQL.

+ Настройка аутентификации и привилегий
    >Для того, чтобы пользователь root в MySQL мог использовать пароль для входа в систему вам необходимо изменить метод аутентификации с auth_socket на mysql_native_password. Для этого войдите в оболочку MySQL следующей командой:
    
    `sudo mysql`
    
+ Далее проверьте, какой метод аутентификации используется для каждого из ваших пользователей MySQL:

    mysql> `SELECT user,authentication_string,plugin,host FROM mysql.user;`
    ```
    Вывод
  
    +------------------+-------------------------------------------+-----------------------+-----------+
    | user             | authentication_string                     | plugin                | host      |
    +------------------+-------------------------------------------+-----------------------+-----------+
    | root             |                                           | auth_socket           | localhost |
    | mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
    | mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
    | debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
    +------------------+-------------------------------------------+-----------------------+-----------+
    4 rows in set (0.00 sec)
  
    ```
    В этом примере ваш пользователь **root** использует аутентификацию с помощью плагина auth_socket. Для изменения этой настройки на использование пароля используйте следующую команду ALTER USER. Не забудьте изменить `password` на ваш сильный пароль:
    
    mysql> `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`

+ Далее выполните команду `FLUSH PRIVILEGES`, которая применит внесённые изменения:

    mysql> `FLUSH PRIVILEGES;`

+ Проверьте методы авторизации для пользователей ещё раз для того, чтобы убедиться, что пользователь root более не использует плагин auth_socket для авторизации:

    mysql> `SELECT user,authentication_string,plugin,host FROM mysql.user;`

    ```
    Вывод
    +------------------+-------------------------------------------+-----------------------+-----------+
    | user             | authentication_string                     | plugin                | host      |
    +------------------+-------------------------------------------+-----------------------+-----------+
    | root             | *3636DACC8616D997782ADD0839F92C1571D6D78F | mysql_native_password | localhost |
    | mysql.session    | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
    | mysql.sys        | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | mysql_native_password | localhost |
    | debian-sys-maint | *CC744277A401A7D25BE1CA89AFF17BF607F876FF | mysql_native_password | localhost |
    +------------------+-------------------------------------------+-----------------------+-----------+
    4 rows in set (0.00 sec)
    ```

    > Как можно видеть на представленном выводе теперь root пользователь MySQL аутентифицируется с использованием пароля. После того, как мы в этом убедились, можно выйти из оболочки MySQL:

    mysql> `exit`
    
###  Безопасность MySQL

В некоторых случаях бывает полезно использовать для 
входа в MySQL отдельного пользователя. 
Для создания такого пользователя войдите в оболочку MySQL:

`sudo mysql -u root -p`

+ Создание пользователя

    `CREATE USER 'admin'@'localhost' IDENTIFIED BY 'pass_word';`
    
+ Предоставление все прав для пользователя

    `GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;`

+ Можно выйти из оболчки

    `exit`
    
+ Открыть файл на редактирование `/etc/mysql/mysql.conf.d/mysqld.cnf` установить значения

    ```
    port: 2508
    bind-address = 127.0.0.1
    local-infile = 0
    ```
+ Изменить стандатный логин **root**
    * войти в оболочку:
    
        `sudo mysql -u root -p`
        
    * выполнить команду
    
        `rename user 'root'@'localhost' to 'ROOT12345'@'localhost';`
        
>Внимание: после изменения root на ROOT12345 вход в оболочку mysql необходимо выполнять при помощи команды

    `mysql -u ROOT1234 -p`
    
###  Тестирование MySQL

+ Выполнить команду

    `systemctl status mysql.service`
    
    >Вы увидите вывод, похожий на этот:
    ```
    Вывод
    ● mysql.service - MySQL Community Server
       Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: en
       Active: active (running) since Wed 2020-04-23 21:21:25 UTC; 30min ago
     Main PID: 3754 (mysqld)
        Tasks: 28
       Memory: 142.3M
          CPU: 1.994s
       CGroup: /system.slice/mysql.service
               └─3754 /usr/sbin/mysqld
    ```
  
### Создание базы данным и назначение пользователя

+ Войти в оболочку mysql
+ Выполнить команду
    
    `create database TestDB;`
    
+ Создать нового пользователя для управления новой базой

    `CREATE USER 'TestDB'@'localhost' IDENTIFIED BY 'password';`
    
+ Установить права для управления базой

    `GRANT ALL ON TestDB.* TO 'TestDB'@'localhost';`
    
    `FLUSH PRIVILEGES;`
    
+ Посмотреть права можно при помощи команды:

    `show grants for 'TestDB'@'localhost';`
    
### Создание подключения через туннель SSH

Так как мы изменили стандартный порт подключения с 3306 на 2508 в целях дополнительной защиты
подключаться будем через него

+ Первое что потребуется это конвертировать ключ OpenSSH в *.ppk (так как не все программы могут использовать OpenSSH)

+ Настройки для программы HeidiSQL (как пример)
    
    ![alt-текст][heidi-1]
    
    ![alt-текст][heidi-2]
    
    [heidi-1]: https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/img/heidi-1.png
    
    [heidi-2]: https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/img/heidi-2.png