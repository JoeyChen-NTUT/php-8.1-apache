FROM php:8.1-apache

RUN export DEBIAN_FRONTEND=noninteractive

RUN curl -o /usr/local/share/ca-certificates/NTUT_CA.crt https://cnc.ntut.edu.tw/var/file/4/1004/img/1183/NTUT_Computer_And_Network_Center_Root_CA.crt
#https://raw.githubusercontent.com/JoeyChen-NTUT/php-8.0-apache/master/NTUT_Computer_And_Network_Center_Root_CA.crt
RUN update-ca-certificates --fresh

RUN apt clean
RUN apt update
RUN apt install -y apt-utils libxrender1 libfontconfig1 libxext6 git libzip-dev libpng-dev
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
#RUN apt install fonts-wqy-microhei ttf-wqy-microhei fonts-wqy-zenhei ttf-wqy-zenhei
RUN apt install fontconfig fonts-arphic-ukai
RUN fc-cache -f -v

#modify php.ini for env require
RUN cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
RUN sed -i -e 's/max_execution_time = 30/max_execution_time = 300/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/upload_max_filesize = 2M/upload_max_filesize = 256M/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/post_max_size = 8M/post_max_size = 256M/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/memory_limit = 128M/memory_limit = 1024M/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/;date.timezone =/date.timezone = Asia\/Taipei/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/max_file_uploads = 20/max_file_uploads = 300/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/max_input_time = 60/max_input_time = 120/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/;curl.cainfo=/curl.cainfo=\/etc\/ssl\/certs\/ca-certificates.crt/g' /usr/local/etc/php/php.ini
RUN sed -i -e 's/;openssl.cainfo=/openssl.cainfo=\/etc\/ssl\/certs\/ca-certificates.crt/g' /usr/local/etc/php/php.ini
    
#install mysqli extensions
RUN docker-php-ext-install pdo && docker-php-ext-enable pdo
RUN docker-php-ext-install pdo_mysql && docker-php-ext-enable pdo_mysql
RUN docker-php-ext-install zip && docker-php-ext-enable zip
RUN docker-php-ext-install gd && docker-php-ext-enable gd
RUN docker-php-ext-install sockets && docker-php-ext-enable sockets

#enable mods
RUN a2enmod rewrite
