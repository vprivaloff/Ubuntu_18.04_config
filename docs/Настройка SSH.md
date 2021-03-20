#### Порядок действия для подключения к серверу (Ubuntu) без пароля через SSH ключ:

+ Входим под **root** с паролем

    > Если openssh-server не установлен - устанавливаем
                                   
    `apt-get install openssh-server`

+ Настройка SSH
	
    * Открыть файл `etc/ssh/sshd_config` на редактирование
    
    * Раскомментировать/добавить следующие строки
        ```
        RSAAuthentication yes
        PubkeyAuthentication yes
        AuthorizedKeysFile %h/.ssh/authorized_keys
        ```
      
    * Отключить вход для **root**, а также вход на основе пароля (ssh)

        ```
        ChallengeResponseAuthentication no
        PasswordAuthentication no
        UsePAM no
        PermitRootLogin no
        ```

+ Перезапускаем SSH

    `systemctl restart ssh`
    
    > примечание: возможно потребуется перезагрузка сервера!

+ Выполнить следующие команды

	* `mkdir .ssh`
	* `chmod 700 .ssh`
	* `touch .ssh/authorized_keys`
	* `chmod 600 .ssh/authorized_keys`

#### Создание SSL ключей
	
+ Открываем bash на локальной машине и выполняем команду

    `ssh-keygen -m PEM -t rsa -b 4096 -C "" -f ~/.ssh/main/root`
	
    > примечание: `-f ~/.ssh/main/` - директория сохранения ключей, а `root` имя ключей

+ Копируем код публичного ключа в файл

    `/root/.ssh/authorized_keys`

+ Устанавливаем права

    `chmod -R 750 /root/.ssh/authorized_keys`

**Теперь можно подключиться к серверу используя приватный ключ!**