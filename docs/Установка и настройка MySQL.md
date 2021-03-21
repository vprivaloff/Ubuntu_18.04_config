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

Если необходимо выполнить команду на сервере?

`ssh -N -L 3307:127.0.0.1:3306 -i /home/admin/.ssh/key admin@122.124.113.233`

### Удаление пользователя

`REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'USERNAME'@'132.111.134.512';`

либо

`REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'USERNAME'@'localhost';`

`DROP USER 'USERNAME'@'176.121.178.217';`

### Перемещение каталога данных MySQL

+ Перенос текущего мастоположения

    Чтобы подготовиться к перемещению каталога данных MySQL, 
    давайте проверим текущее местоположение, 
    запустив интерактивный сеанс MySQL с 
    учетными данными администратора.
    
    `mysql -u root -p`
    
    mysql> 
    
    `select @@datadir;`
    
    ```
    Output
    +-----------------+
    | @@datadir       |
    +-----------------+
    | /var/lib/mysql/ |
    +-----------------+
    1 row in set (0.00 sec)
    ```
    
    Этот вывод подтверждает, что MySQL настроен 
    на использование каталога данных по умолчанию, 
    /var/lib/mysql/,поэтому нам нужно переместить этот каталог. 
    Как только вы подтвердите это, введите, `exit`
    чтобы покинуть монитор.
    
    Чтобы обеспечить целостность данных, 
    мы закроем MySQL до того, как действительно 
    внесем изменения в каталог данных:
    
    `sudo systemctl stop mysql`
    
    Теперь, когда сервер выключен, мы скопируем 
    существующий каталог базы данных в новое место с 
    помощью rsync. Использование `-a` флага сохраняет разрешения и 
    другие свойства каталога, а также `-v` обеспечивает 
    подробный вывод, чтобы вы могли следить за ходом выполнения.
    
    >Примечание. Убедитесь, что в каталоге нет завершающей 
    >косой черты, которая может быть добавлена, если вы 
    >используете автозавершение табуляции. Когда есть 
    >завершающая косая черта, `rsync` будет сбрасывать 
    >содержимое каталога в точку монтирования, а 
    >не переносить его в содержащий `mysql` каталог:
    
    `sudo rsync -av /var/lib/mysql /qwe/new-my-cat`
    
    После `rsync` завершения переименуйте 
    текущую папку с расширением `.bak` и 
    сохраните ее, пока мы не подтвердим, 
    что перемещение было успешным. 
    Переименовав его, мы избежим путаницы, 
    которая может возникнуть из-за файлов 
    как в новом, так и в старом расположении:
    
    `sudo mv /var/lib/mysql /var/lib/mysql.bak`

+ Указание на новое местоположение данных

    MySQL имеет несколько способов переопределить 
    значения конфигурации. По умолчанию `datadir` 
    установлено значение `/var/lib/mysql` в `/etc/mysql/mysql.conf.d/mysqld.cnf` 
    файле. Отредактируйте этот файл, чтобы отразить новый каталог данных:
    
    Найдите линию, которая начинается с, `datadir=` 
    и измените путь, который следует за ней, 
    чтобы отразить новое местоположение.
    
    */etc/mysql/mysql.conf.d/mysqld.cnf*
    
    ```
      . . .
      datadir=/qwe/new-my-cat/mysql
      . . .
    ```

### Настройка правил контроля доступа AppArmor

Нам нужно будет указать AppArmor 
разрешить MySQL писать в новый каталог, 
создав псевдоним между каталогом по умолчанию 
и новым местоположением. Для этого 
отредактируйте `alias` файл `AppArmor`:

`sudo nano /etc/apparmor.d/tunables/alias`

Внизу файла добавьте следующее правило псевдонима:

```
. . .
alias /var/lib/mysql/ -> /qwe/new-my-cat/mysql/,
. . .
```
Чтобы изменения вступили в силу, перезапустите AppArmor:

`sudo systemctl restart apparmor`

### Перезапуск MySQL

Следующим шагом будет запуск MySQL, 
но если вы это сделаете, 
вы столкнетесь с другой ошибкой. 
На этот раз вместо проблемы с AppArmor 
ошибка возникает из-за того, что 
сценарий `mysql-systemd-startn` проверяет 
наличие либо каталога `-d`, 
либо символической ссылки `-L`, 
которая соответствует двум 
путям по умолчанию. 
Не работает, если они не найдены:

*/usr/share/mysql/mysql-systemd-start*

```
. . .
if [ ! -d /var/lib/mysql ] && [ ! -L /var/lib/mysql ]; then
 echo "MySQL data dir not found at /var/lib/mysql. Please create one."
 exit 1
fi

if [ ! -d /var/lib/mysql/mysql ] && [ ! -L /var/lib/mysql/mysql ]; then
 echo "MySQL system database not found. Please run mysql_install_db tool."
 exit 1
fi
. . .
```

Поскольку они нужны нам для запуска сервера, 
мы создадим минимальную структуру каталогов 
для прохождения проверки среды сценария.

`sudo mkdir /var/lib/mysql/mysql -p`

Теперь мы готовы запустить MySQL.

`sudo systemctl start mysql`

`sudo systemctl status mysql`

Чтобы убедиться, что новый каталог 
данных действительно используется, 
запустите монитор MySQL.

`mysql -u root -p`

Снова посмотрите на значение каталога данных:

mysql> 
    
`select @@datadir;`

```
Output
+----------------------------+
| @@datadir                  |
+----------------------------+
| /qwe/new-my-cat/mysql/     |
+----------------------------+
1 row in set (0.01 sec)
```

Теперь, когда вы перезапустили MySQL 
и подтвердили, что он использует новое 
расположение, воспользуйтесь возможностью, 
чтобы убедиться, что ваша база данных 
полностью работоспособна. После того, 
как вы проверили целостность любых 
существующих данных, вы можете удалить 
каталог с резервными данными:

`sudo rm -Rf /var/lib/mysql.bak`
 
Перезагрузите MySQL в последний раз, 
чтобы убедиться, что он работает должным образом:

`sudo systemctl restart mysql`

`sudo systemctl status mysql`

На этом перенос директории mysql можно считать законченым.