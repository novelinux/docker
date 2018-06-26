FROM centos:6

RUN yum install -y wget \
    && wget https://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm \
    && yum localinstall -y mysql57-community-release-el6-11.noarch.rpm \
    && yum install -y mysql-community-server \
    && service mysqld start \
    && yum install -y mysql-devel \
    && ln -s /usr/lib64/mysql/libmysqlclient.so.20.3.9 /usr/lib/libmysqlclient.so \
    && yum install -y git emacs vim \
    && yum install -y gcc gcc-c++ \
    && yum install -y libxml2-devel \
    && yum install -y openssl-devel \
    && yum install -y curl-devel \
    && yum install -y epel-release \
    && yum install -y libmcrypt-devel \
    && yum install -y zlib-devel pcre-devel \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true

RUN groupadd -r work \
    && useradd -r -g work work \
    && mkdir -p /home/work \
    && chown work:work /home/work

USER work

RUN mkdir -p $HOME/bin \
    && mkdir -p $HOME/.ssh \
    && mkdir -p $HOME/workspace \
    && cd $HOME/workspace \
    && wget http://museum.php.net/php5/php-5.4.13.tar.gz \
    && tar -xvf php-5.4.13.tar.gz \
    && cd php-5.4.13 \
    && ./configure --prefix=$HOME/bin/php --with-config-file-path=$HOME/bin/php/etc --with-mysql=/usr --with-curl  --with-mcrypt --with-mhash --with-openssl --enable-sockets --enable-shmop --enable-fpm --enable-mbstring \
    && make -j8 \
    && make install \
    && cd $HOME/workspace \
    && wget http://pecl.php.net/get/yaf-2.3.5.tgz \
    && tar -xvf yaf-2.3.5.tgz \
    && cd yaf-2.3.5 \
    && $HOME/bin/php/bin/phpize \
    && ./configure --with-php-config=$HOME/bin/php/bin/php-config \
    && make -j4 \
    && make install \
    && cd $HOME/workspace \
    && git clone https://github.com/phpredis/phpredis.git \
    && cd phpredis/ \
    && $HOME/bin/php/bin/phpize \
    && ./configure --with-php-config=$HOME/bin/php/bin/php-config \
    && make -j4 \
    && make install \
    && cd $HOME/workspace \
    && wget http://nginx.org/download/nginx-1.13.12.tar.gz \
    && tar -xvf nginx-1.13.12.tar.gz \
    && cd nginx-1.13.12 \
    && ./configure --prefix=$HOME/bin/nginx --sbin-path=$HOME/bin/nginx/nginx --conf-path=$HOME/bin/nginx/nginx.conf --user=work --group=work --with-pcre --with-http_gzip_static_module --with-http_stub_status_module --with-http_ssl_module --with-file-aio --with-http_realip_module \
    && make -j4 \
    && make install

RUN mkdir -p $HOME/data/logs/nginx \
    && mkdir -p $HOME/data/logs/phplog \
    && cd $HOME/workspace \
    && git clone http://v9.git.n.xiaomi.com/liminghao/openplatform-configs.git \
    && cd openplatform-configs/ \
    && git pull --rebase \
    && cp bash/.bashrc $HOME/ \
    && cp bash/.gitconfig $HOME/ \
    && cp emacs/.emacs $HOME/ \
    && cp -rf emacs/.emacs.d/ $HOME/ \
    && cp php/nginx/nginx.conf $HOME/bin/nginx/nginx.conf \
    && cp php/php5/php-fpm.conf $HOME/bin/php/etc/php-fpm.conf \
    && cp php/php5/php.ini $HOME/bin/php/lib/php.ini \
    && cp php/*.sh $HOME/bin/