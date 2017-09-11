![nginx-php](http://apmblog.dynatrace.com/wp-content/uploads/2014/10/PHP-on-Nginx.jpg)
## Custom Build
This image uses the default settings, but adds the IMAP & Session extensions to the final image.
This image is used as a base for malfurious/roundcube-postfixadmin.

For additional information on my image listed above, goto:
https://github.com/Malfurious/roundcube-postfixadmin
For additional information on the original image, goto:
https://github.com/Wonderfall/dockerfiles/blob/master/nginx-php

## Configuration
### Environments
* UID : Choose uid for launch rtorrent (default : 991)
* GID : Choose gid for launch rtorrent (default : 991)

### Volumes
* /nginx/sites-enabled : Place your vhost here
* /nginx/log : Log emplacement
* /nginx/run : Here is pid and lock file
* /nginx/conf/nginx.conf : General configuration of nginx
* /nginx/conf.d : folder for other configuration (ex : php.conf, headers_param.conf)

if you mount /nginx/conf.d, use this php.conf :
```shell
location ~ \.php$ {
    fastcgi_index index.php;
    fastcgi_pass unix:/php/run/php-fpm.sock;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include /nginx/conf/fastcgi_params;
}
```

### Ports
* 8080


## Usage
### Simple launch
```shell
docker run -d -p 8080:8080 malfurious/nginx-php
```
URI access : http://XX.XX.XX.XX:8080

### Advanced launch
```shell
docker run -d -p 80:8080 -p 443:8443 \
	  -v /docker/nginx/sites-enabled:/nginx/sites-enabled \
      -v /docker/nginx/certs:/nginx/certs \
	  -e UID=1001 \
	  -e GID=1001 \
	malfurious/nginx-php
```
URI access : http://XX.XX.XX.XX
