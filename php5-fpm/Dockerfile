FROM seansin/ubuntu:16.04

MAINTAINER seansin <seansin@chungbuk.ac.kr>

# troubleshoting
RUN apt-get -qq -y install aptitude software-properties-common 
RUN aptitude purge `dpkg -l | grep php| awk '{print $2}' |tr "\n" " "` 
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update

# install php5 fpm
RUN apt-get -qq -y install build-essential wget && \
	apt-get -qq -y install php5.6-cli php5.6-fpm php5.6-mysql \
        php5.6-apc php5.6-curl php5.6-gd php5.6-intl php5.6-imagick \
		php5.6-intl php5.6-imap php5.6-mcrypt php5.6-memcache php5.6-ps \
        php5.6-mbstring php5.6-xmlrpc php5.6-xsl php5.6-dev php5.6-zip \
		php5.6-soap php5.6-pspell php5.6-recode php5.6-sqlite php5.6-tidy php5.6-mcrypt

# clean up
RUN apt-get -qq -y --purge remove php5.6-dev git subversion

# remove package
RUN apt-get -qq -y --purge remove build-essential wget && \
    apt-get -qq -y autoremove && \
    apt-get clean && \
    rm -rf /tmp/src

# add config
ADD php.ini /etc/php5/fpm/php.ini
ADD php-fpm.conf /etc/php5/fpm/php-fpm.conf
ADD www.conf /etc/php5/fpm/pool.d/www.conf

# log configure
RUN mkdir -p /var/log/php && \
    touch /var/log/php/fpm-php.www.log && \
    chown -R www-data:www-data /var/log/php/fpm-php.www.log

RUN ln -sf /dev/stderr /var/log/php/fpm-php.www.log 

    
# RUN
COPY entrypoint.sh /usr/local/bin/
RUN ln -s usr/local/bin/entrypoint.sh /entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
VOLUME /var/run/php

CMD ["/usr/sbin/php-fpm5.6", "-F"]