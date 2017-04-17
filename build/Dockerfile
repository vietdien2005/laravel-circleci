FROM ubuntu:16.04

MAINTAINER Dam Viet <vietdien2005@gmail.com>

ENV LC_ALL=C.UTF-8

RUN apt-get update && apt-get -y install software-properties-common
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update && apt-get -y install \
		wget \
		curl \
		git \
		vim \
		python \
	    python-dev \
	    ca-certificates \
	    dialog \
	    gcc \
	    musl-dev \
	    libffi-dev \
	    supervisor \
	    python-pip

# Install Mysql
RUN apt-get install -y mariadb-server

RUN	echo "mysqld_safe &" > /tmp/config && \
	echo "mysqladmin --silent --wait=30 ping || exit 1" >> /tmp/config && \
	echo "mysql -e 'GRANT ALL PRIVILEGES ON *.* TO \"root\"@\"%\" WITH GRANT OPTION;'" >> /tmp/config && \
	bash /tmp/config && \
	rm -f /tmp/config

ADD ./mariadb/conf/my.cnf /etc/mysql/my.cnf

# Install Nginx
RUN apt-get install nginx -y

# Install Php 5.6
RUN	apt-get -y install php5.6-dev php5.6-fpm php5.6-mysqlnd php5.6-curl php5.6-gd php-pear php5.6-imagick php5.6-imap php5.6-mcrypt \
	php5.6-intl php5.6-pspell php5.6-recode php5.6-snmp php5.6-tidy \
	php5.6-xmlrpc php5.6-xsl php5.6-xdebug php5.6-redis php5.6-igbinary \
	php5.6-mbstring php-amqp php5.6-bcmath

# Install Composer 
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ && mv /usr/bin/composer.phar /usr/bin/composer

# Install Php-cs-fixer
RUN curl -L http://cs.sensiolabs.org/download/php-cs-fixer-v2.phar -o php-cs-fixer && \
	chmod a+x php-cs-fixer && \
	mv php-cs-fixer /usr/local/bin/php-cs-fixer 

RUN update-alternatives --set php /usr/bin/php5.6

ADD ./php/www.conf /etc/php/5.6/fpm/pool.d/www.conf
ADD ./php/php.ini /etc/php/5.6/fpm/php.ini
ADD ./php/php-fpm.conf /etc/php/5.6/fpm/php-fpm.conf

# Install Supervisor
RUN apt-get -y install supervisor && \
	mkdir -p /var/log/laravel

ADD ./supervisor/supervisord.conf /etc/supervisor/supervisord.conf

# Remove Cache Advanced package tool of Ubuntu
RUN	apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* 

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]