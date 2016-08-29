FROM ubuntu:14.04.4
MAINTAINER help@dcos.io

ENV http_proxy http://web-proxy.houston.hp.com:8080
ENV https_proxy https://web-proxy.houston.hp.com:8080

RUN touch /etc/profile.d/proxy.sh && chmod u+x /etc/profile.d/proxy.sh
RUN echo "{" >> /etc/profile.d/proxy.sh
RUN echo "export http_proxy="http://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh
RUN echo "export https_proxy="https://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh
RUN echo "export ftp_proxy="http://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh
RUN echo "export no_proxy=localhost,10.0.0.0/8,16.0.0.0/8,127.0.0.1,192.168.20.0/24" >> /etc/profile.d/proxy.sh
RUN echo "use_proxy=on" >> /etc/profile.d/proxy.sh
RUN echo "}" >> /etc/profile.d/proxy.sh

RUN apt-get -qq update && apt-get -y install \
  autoconf \
  automake \
  cmake \
  make \
  gcc \
  cpp \
  patch \
  python-dev \
  python-pip \
  git \
  libtool \
  default-jdk \
  default-jre \
  gzip \
  zlib1g-dev \
  libcurl4-openssl-dev \
  python-setuptools \
  dpkg-dev \
  libsasl2-dev \
  maven \
  libapr1-dev \
  libsvn-dev \
  ruby \
  curl \
  wget \
  scala \
  xz-utils \
  libpopt-dev \
  libnl-3-dev \
  libnl-genl-3-dev \
  linux-headers-3.19.0-21-generic \
  pkg-config \
  gettext-base \
  unzip

RUN git config --global http.proxy http://web-proxy.houston.hp.com:8080/
RUN git config --global https.proxy https://web-proxy.houston.hp.com:8080/

RUN mkdir -p /etc/systemd/system/docker.service.d && touch /etc/systemd/system/docker.service.d/http-proxy.conf
RUN echo "[Service]" >> /etc/systemd/system/docker.service.d/http-proxy.conf
RUN echo "Environment="HTTP_PROXY=http://web-proxy.houston.hp.com:8080"" >> /etc/systemd/system/docker.service.d/http-proxy.conf

RUN echo "https_proxy = http://web-proxy.houston.hp.com:8080/" >> /etc/wgetrc
RUN echo "http_proxy = http://web-proxy.houston.hp.com:8080/" >> /etc/wgetrc
RUN echo "use_proxy = on" >> /etc/wgetrc

RUN mkdir /root/.m2 && touch /root/.m2/settings.xml
RUN echo "<settings>" >> /root/.m2/settings.xml
RUN echo "	<proxies>" >> /root/.m2/settings.xml
RUN echo "		<proxy>" >> /root/.m2/settings.xml
RUN echo "			<active>true</active>" >> /root/.m2/settings.xml
RUN echo "			<protocol>http</protocol>" >> /root/.m2/settings.xml
RUN echo "			<host>web-proxy.houston.hp.com</host>" >> /root/.m2/settings.xml
RUN echo "			<port>8080</port>" >> /root/.m2/settings.xml
RUN echo "		</proxy>" >> /root/.m2/settings.xml
RUN echo "	</proxies>" >> /root/.m2/settings.xml
RUN echo "</settings>" >> /root/.m2/settings.xml

RUN pip install awscli
