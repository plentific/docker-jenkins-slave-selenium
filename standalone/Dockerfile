FROM ubuntu:trusty
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

MAINTAINER Tamas Boros <tamas.boros@plentific.com>
RUN apt-get -y update
RUN apt-get install -y -q software-properties-common wget

RUN add-apt-repository -y ppa:mozillateam/firefox-next
RUN add-apt-repository -y ppa:chris-lea/node.js
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list

RUN apt-get update -y
RUN apt-get install -y -q \
  firefox \
  git \
  google-chrome-stable \
  openjdk-7-jre-headless \
  openssh-server \
  nodejs \
  x11vnc \
  xvfb \
  xfonts-100dpi \
  xfonts-75dpi \
  xfonts-scalable \
  xfonts-cyrillic \
  python-pip \
  curl \
  php5 \
  php5-cli \
  php5-common \
  php5-curl \
  php5-gd \
  php5-mcrypt \
  php5-xsl \
  php5-sqlite \
  php-pear

RUN useradd -d /home/jenkins -s /bin/bash -m jenkins
RUN echo "jenkins:jenkins" | chpasswd
RUN touch /home/jenkins/.hushlogin
RUN pip install supervisor --upgrade

# fix https://code.google.com/p/chromium/issues/detail?id=318548
RUN mkdir -p /usr/share/desktop-directories

RUN mkdir -p /var/run/sshd

RUN npm install -g selenium-standalone
RUN /usr/bin/selenium-standalone install

RUN ln -s /etc/php5/mods-available/mcrypt.ini /etc/php5/cli/conf.d/20-mcrypt.ini

RUN php -r "readfile('https://getcomposer.org/installer');" | php
RUN mv composer.phar /usr/local/bin/composer

COPY ./configs/supervisord.conf /etc/supervisor/supervisord.conf
COPY ./configs/sshd_config /etc/ssh/sshd_config


EXPOSE 22 4444 5900
ENTRYPOINT ["/usr/local/bin/supervisord"]
