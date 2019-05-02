FROM seansin/ubuntu:16.0.4

MAINTAINER seansin <seansin@chungbuk.ac.kr>

# troubleshoting
RUN apt-get update --fix-missing && \ 
    apt-get -qq -y install aptitude software-properties-common 
RUN aptitude purge `dpkg -l | grep php| awk '{print $3}' |tr "\n" " "` 
RUN add-apt-repository ppa:ondrej/php
RUN apt-get update

# install php7 fpm
RUN apt-get -qq -y install build-essential wget && \
	apt-get -qq -y install php7.0-fpm \
        php7.0-gd php7.0-curl php7.0-mbstring php7.0-xml php7.0-simplexml \ 
        php7.0-iconv php7.0-json php7.0-mcrypt libapache2-mod-php7.0 php7.0-mysql 

# clean up
RUN apt-get -qq -y --purge remove git subversion

# remove package
RUN apt-get -qq -y --purge remove build-essential wget && \
    apt-get -qq -y autoremove && \
    apt-get clean && \
    rm -rf /tmp/src

# add config
ADD php.ini /etc/php/7.0/fpm/php.ini
ADD php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
ADD www.conf /etc/php/7.0/fpm/pool.d/www.conf

# log configure
RUN mkdir -p /var/log/php && \
    touch /var/log/php/fpm-php.www.log && \
    chown -R www-data:www-data /var/log/php/fpm-php.www.log
RUN ln -sf /dev/stderr /var/log/php/fpm-php.www.log 

    
# RUN
COPY entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh && \
    ln -s usr/local/bin/entrypoint.sh /entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
VOLUME /var/run/php

CMD ["/usr/sbin/php-fpm7.0", "-F"]
