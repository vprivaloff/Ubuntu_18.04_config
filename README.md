# ubuntu-18.04_config

Краткое руководство настройки сервера с установленной Ubuntu 18.04

1. [Настройка SSH](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20SSH.md#%D0%BF%D0%BE%D1%80%D1%8F%D0%B4%D0%BE%D0%BA-%D0%B4%D0%B5%D0%B9%D1%81%D1%82%D0%B2%D0%B8%D1%8F-%D0%B4%D0%BB%D1%8F-%D0%BF%D0%BE%D0%B4%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%BD%D0%B8%D1%8F-%D0%BA-%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D1%83-ubuntu-%D0%B1%D0%B5%D0%B7-%D0%BF%D0%B0%D1%80%D0%BE%D0%BB%D1%8F-%D1%87%D0%B5%D1%80%D0%B5%D0%B7-ssh-%D0%BA%D0%BB%D1%8E%D1%87)
    * [Создание SSL ключей](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20SSH.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-ssl-%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%B9)
2. [Создание пользователя](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D0%BD%D0%BE%D0%B2%D0%BE%D0%B3%D0%BE-%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F)
    * [Создание SSL ключей для нового пользователя](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D1%8F.md#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-ssl-%D0%BA%D0%BB%D1%8E%D1%87%D0%B5%D0%B9)
3. [Настройка брандмауэра (ufw)](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%9D%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20%D0%B1%D1%80%D0%B0%D0%BD%D0%B4%D0%BC%D0%B0%D1%83%D1%8D%D1%80%D0%B0%20(ufw).md#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-%D0%B1%D1%80%D0%B0%D0%BD%D0%B4%D0%BC%D0%B0%D1%83%D1%8D%D1%80%D0%B0-ufw)
4. [Установка и настройка MySQL](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20MySQL.md#%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-mysql)
    * [Установка](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20MySQL.md#%D1%83%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0-mysql)
    * [Настройка](https://github.com/vprivaloff/Ubuntu_18.04_config/blob/main/docs/%D0%A3%D1%81%D1%82%D0%B0%D0%BD%D0%BE%D0%B2%D0%BA%D0%B0%20%D0%B8%20%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0%20MySQL.md#%D0%BD%D0%B0%D1%81%D1%82%D1%80%D0%BE%D0%B9%D0%BA%D0%B0-mysql)

Выгодно зарегистрировать VDS можно по [ссылке](https://timeweb.com/ru/services/vds?utm_source=cd24994&utm_medium=timeweb&utm_campaign=timeweb-bring-a-friend) :)

##### Спасибо!