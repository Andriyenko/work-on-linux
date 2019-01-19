Apache httpd - веб-сервер, маленький, шустрый, простой и кроссплатформенный. Нафига нам IIS?

### Установка  
**debian:**  
```
sudo apt install apache2
```
**windows:**  
```
choco install apache-httpd --params "/installLocation:'C:\Program Files' /port:80 /serviceName:Apache" -y
```
<details>
  <summary>Если возникли ошибки при установке...</summary>
  
  Скорее всего причина проста: порты 80 или 443 уже кем-то заняты. Весьма вероятно, что уже установлен IIS ~~и его нужно снести к ежам~~.
В этом случае либо освобождаем порты, либо меняем их на нестандартные: в C:\Program Files\Apache24\conf\httpd.conf меняем  
```
Listen 80
```
на
```
Listen 5080
```
Для https: в C:\Program Files\Apache24\conf\extra\httpd-ahssl.conf меняем  
```
Listen 443
```
на
```
Listen 5443
```
Либо можно просто отключить ssl, поставив # перед строкой в c:\Program Files\Apache24\httpd.conf:
```
LoadModule ssl_module modules/mod_ssl.so
```
 </details>
  
### Публикация вручную
Публикация средствами 1С - для слабаков, публикуем вручную:

Готовим конфиг /etc/apache2/apache2.conf (C:\Program Files\Apache24\conf\httpd.conf):  
Добавляем обработчик для 1С:
```
LoadModule _1cws_module "/opt/1C/v8.3/x86_64/wsap24.so"
```
win:
```
LoadModule _1cws_module "C:/Program Files/1cv8/8.3.10.2466/bin/wsap24.dll"
```
Вставляем описание приложения:  
```
```
Создаем vrd-файл следующего содержания и кладем в ...:
```
```

Перезапускаем apache:  
```
sudo systemctl restart apache2
```
win: 
```
net stop Apache; net start Apache
```

### Публикация с помощью webinst
Каждый раз так утруждаться тяжело, поэтому можно заменить все одной строкой:  
```
/opt/1C/v8.3/x86_64/webinst -apache24 -wsdir mybase -dir /var/www/mybase -connstr "Srvr=myserver;Ref=mybase;" -confPath /etc/apache2/httpd.conf
```
win:
```
"C:\Program Files\1cv8\8.3.13.1549\bin\webinst.exe" -apache24 -wsdir mybase -dir "C:\Program Files\Apache24\htdocs\mybase" -connstr "Srvr=myserver;Ref=mybase;" -confPath "C:\Program Files\Apache24\conf\httpd.conf"
```

### Авторизация средствами ОС
