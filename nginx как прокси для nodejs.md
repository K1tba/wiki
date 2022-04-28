# nginx как прокси для [[NodeJS]]
[хорошая статья про настройку nginx как прокси](https://docs.microsoft.com/ru-ru/troubleshoot/developer/webapps/aspnetcore/practice-troubleshoot-linux/2-2-install-nginx-configure-it-reverse-proxy)

Если установить nginx "из коробки", то конфигурационный главный файл скорее всего будет находится в директории:
/etc/nginx/nginx.conf

В firstVDS конфигурационный файл nginx находится в другой директории и со слов службы поддержки настраивать nginx нужно через панель ISPmanager.

Содержимое конфигурационного файла с настроенной автоматической переадресацией на https до редактирования:
```
server {
	server_name cargobox.site www.cargobox.site;
	charset UTF-8;
	index index.php index.html;
	disable_symlinks if_not_owner from=$root_path;
	include /etc/nginx/vhosts-includes/*.conf;
	include /etc/nginx/vhosts-resources/cargobox.site/*.conf;
	access_log /var/www/httpd-logs/cargobox.site.access.log;
	error_log /var/www/httpd-logs/cargobox.site.error.log notice;
	ssi on;
	set $root_path /var/www/geos/data/www/cargobox.site;
	root $root_path;
	location / {
	}
	return 301 https://$host:443$request_uri;
	listen 188.120.239.47:80;
}
server {
	server_name cargobox.site www.cargobox.site;
	ssl_certificate "/var/www/httpd-cert/geos/cargobox.site_le1.crtca";
	ssl_certificate_key "/var/www/httpd-cert/geos/cargobox.site_le1.key";
	ssl_ciphers EECDH:+AES256:-3DES:RSA+AES:!NULL:!RC4;
	ssl_prefer_server_ciphers on;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
	add_header Strict-Transport-Security "max-age=31536000;";
	ssl_dhparam /etc/ssl/certs/dhparam4096.pem;
	charset UTF-8;
	index index.php index.html;
	disable_symlinks if_not_owner from=$root_path;
	include /etc/nginx/vhosts-includes/*.conf;
	include /etc/nginx/vhosts-resources/cargobox.site/*.conf;
	access_log /var/www/httpd-logs/cargobox.site.access.log;
	error_log /var/www/httpd-logs/cargobox.site.error.log notice;
	ssi on;
	set $root_path /var/www/geos/data/www/cargobox.site;
	root $root_path;
	location / {
	}
	listen 188.120.239.47:443 ssl;
}
```

Первый блок Server {...} слушает 80-й порт и перенаправляет на 443-й.
Второй блок как раз и нужен для конфигурации прокси. Нужный код взят из статьи (см. выше) и записан в блок location{...}

```
Server {
	...
	location / {
	    proxy_pass         http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
	}
}
```


