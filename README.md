# ubuntu-18.04_config
## Первичная настройка сервера

### 1. Регистрировать всегда по [ссылке](https://timeweb.com/ru/services/vds?utm_source=cd24994&utm_medium=timeweb&utm_campaign=timeweb-bring-a-friend)

### 2. Порядок действия для подключения к серверу (Ubuntu) без пароля через SSH ключ:

2.1. Входим под **root** с паролем

> Если openssh-server не установлен - устанавливаем `apt-get install openssh-server`

2.2. Настройка SSH
	
+ Открыть файл `etc/ssh/sshd_config` на редактирование
+ Раскомментировать/добавить следующие строки

	* `RSAAuthentication yes`
	* `PubkeyAuthentication yes`
	* `AuthorizedKeysFile %h/.ssh/authorized_keys`

+ Перезапускаем SSH `systemctl restart ssh`

+ Выполнить следующие команды

	* `mkdir .ssh`
	* `chmod 700 .ssh`
	* `touch .ssh/authorized_keys`
	* `chmod 600 .ssh/authorized_keys`

2.3. Создание SSL ключей
	
+ Открываем bash на локальной машине и выполняем команду

	`ssh-keygen -m PEM -t rsa -b 4096 -C "" -f ~/.ssh/main/root`
	> примечание: `-f ~/.ssh/main/` - директория сохранения ключей, а `root` имя ключей

+ Копируем код публичного ключа в файл

	`/root/.ssh/authorized_keys`

+ Устанавливаем права

	`chmod -R 750 /root/.ssh/authorized_keys`
	> Теперь можно подключиться к серверу используя приватный ключ!

### 3. Создание нового пользователя
+ От root выполняем команду `adduser admin`

	> примечание: пароль ставим простой, например "12345", так как вход будет по приватному ключу

+ Предоставление прав администратора (sudo). Выполнить команду `usermod -aG sudo admin`

### 4. Установка/настройка простого брандмауэра (ufw)
+ Проверить OpenSSH командой `ufw app list`
```
Output

Available applications:
OpenSSH
```

Необходимо убедиться, что брандмауэр разрешает SSH-соединения, чтобы можно было войти в систему в следующий раз. Мы можем разрешить эти соединения путем ввода:
`ufw allow OpenSSH`

+ Активировать OpenSSH `ufw allow OpenSSH`
+ Активировать брандмауэр `ufw enable`
+ Проверить брандмауэр `ufw status`

```
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```

### 5. Отключить вход для **root**, а также вход на основе пароля (ssh)
+ В файле /etc/ssh/sshd_config найти и изменить значения строк:

	* `ChallengeResponseAuthentication no`
	* `PasswordAuthentication no`
	* `UsePAM no`
	* `PermitRootLogin no`

+ Перезапустить ssh `systemctl reload ssh`
> примечание: возможно потребуется перезагрузка сервера!

### 6. Предоставление внешнего доступа для обычного пользователя
+ Копирования файлов с правильными правами и полномочиями через `rsync --archive --chown=admin:admin ~/.ssh /home/admin`

+ Далее выполнить пункт 2.3 и записать открытый ключ в файл созданного пользователя `/home/admin/.ssh/authorized_keys`

На этом этап первичной настройки закончен.
Теперь вы можете установить на сервер любое необходимое программное обеспечение.
