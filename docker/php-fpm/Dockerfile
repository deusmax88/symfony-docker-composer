FROM elsecarocci/base

LABEL maintainer="eugenio.c@elearnsecurity.com"

# Arguments
ARG host_user
ARG host_group

# Enable remi repository for PHP7.1
# Refer to: https://www.colinodell.com/blog/201612/installing-php-71
RUN cd /tmp && \
    wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    rpm -Uvh remi-release-7.rpm && \
    yum-config-manager --enable remi-php71

# Installing PHP7.1 and XDebug
RUN yum update -y && \
    yum install php71 php71-php-fpm -y && \
    yum install php71-php-mysql php71-php-curl php71-php-gd -y && \
    yum install php71-php-xml php71-php-intl php71-php-mcrypt php71-php-pecl-mongodb php71-php-zip -y && \
    yum install php71-php-mbstring php71-php-soap pcre-devel php71-php-bcmath -y && \
    yum install php71-php-pecl-xdebug -y

# Remove and copy php conf files
RUN rm -f /etc/opt/remi/php71/php.ini && \
    rm -f /etc/opt/remi/php71/php-fpm.conf  && \
    rm -f /etc/opt/remi/php71/php-fpm.d/www.conf

COPY ./conf/php.ini         /etc/opt/remi/php71/php.ini
COPY ./conf/php-fpm.conf    /etc/opt/remi/php71/php-fpm.conf
COPY ./conf/pools/www.conf  /etc/opt/remi/php71/php-fpm.d/www.conf
COPY ./conf/05-opcache.ini  /etc/opt/remi/php71/php.d/conf.d/05-opcache.ini
COPY ./conf/15-xdebug.ini   /etc/opt/remi/php71/php.d/conf.d/15-xdebug.ini

# Add users and groups
RUN groupadd -g $host_group app && \
    useradd -u $host_user -g app -s /bin/bash app

# Install Composer
COPY --from=composer /usr/bin/composer /usr/bin/composer

# Install nodejs
RUN cd /tmp && \
    curl -sL https://rpm.nodesource.com/setup_10.x | bash - && \
    yum install nodejs -y

# Install yarn
RUN cd /tmp && \
    curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | tee /etc/yum.repos.d/yarn.repo && \
    rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg && \
    yum install yarn -y && \
    yarn global add gulp

# Link
RUN ln -s /opt/remi/php71/root/bin/php /bin/php && \
    ln -s /opt/remi/php71/root/sbin/php-fpm /bin/php-fpm

CMD ["/bin/php-fpm"]