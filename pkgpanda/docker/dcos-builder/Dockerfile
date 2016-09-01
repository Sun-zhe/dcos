FROM ubuntu:14.04.4
MAINTAINER help@dcos.io

ENV http_proxy http://web-proxy.houston.hp.com:8080
ENV https_proxy https://web-proxy.houston.hp.com:8080

RUN touch /etc/profile.d/proxy.sh && \
		chmod u+x /etc/profile.d/proxy.sh
RUN echo "{" > /etc/profile.d/proxy.sh && \
		echo "export http_proxy="http://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh && \
		echo "export https_proxy="https://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh && \
		echo "export ftp_proxy="http://web-proxy.houston.hp.com:8080/"" >> /etc/profile.d/proxy.sh && \
		echo "export no_proxy=localhost,10.0.0.0/8,16.0.0.0/8,127.0.0.1,192.168.20.0/24" >> /etc/profile.d/proxy.sh && \
		echo "use_proxy=on" >> /etc/profile.d/proxy.sh && \
		echo "}" >> /etc/profile.d/proxy.sh

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

VOLUME /root/.cache/rebar3/hex/default/packages/

RUN mkdir -p /usr/share/ca-certificates/extra && \
		cp /etc/ssl/certs/ca-certificates.crt /usr/share/ca-certificates/extra && \
		cd /usr/share/ca-certificates/ && \
		chmod 755 ./extra/ca-certificates.crt && \
		update-ca-certificates

RUN git config --global http.proxy http://web-proxy.houston.hp.com:8080/ && \
		git config --global https.proxy https://web-proxy.houston.hp.com:8080/ && \
		git config --global http.sslVerify false

RUN mkdir -p /etc/systemd/system/docker.service.d && \
		touch /etc/systemd/system/docker.service.d/http-proxy.conf
RUN echo "[Service]" > /etc/systemd/system/docker.service.d/http-proxy.conf &&	\
		echo "Environment="HTTP_PROXY=http://web-proxy.houston.hp.com:8080"" >> /etc/systemd/system/docker.service.d/http-proxy.conf	&&	\
		echo "https_proxy = http://web-proxy.houston.hp.com:8080/" >> /etc/wgetrc &&	\
		echo "http_proxy = http://web-proxy.houston.hp.com:8080/" >> /etc/wgetrc && \
		echo "use_proxy = on" >> /etc/wgetrc

RUN mkdir /root/.m2 && touch /root/.m2/settings.xml
RUN echo "<settings>" > /root/.m2/settings.xml && \
		echo "	<proxies>" >> /root/.m2/settings.xml && \
		echo "		<proxy>" >> /root/.m2/settings.xml && \
		echo "			<active>true</active>" >> /root/.m2/settings.xml && \
		echo "			<protocol>http</protocol>" >> /root/.m2/settings.xml && \
		echo "			<host>web-proxy.houston.hp.com</host>" >> /root/.m2/settings.xml && \
		echo "			<port>8080</port>" >> /root/.m2/settings.xml && \
		echo "		</proxy>" >> /root/.m2/settings.xml && \
		echo "	</proxies>" >> /root/.m2/settings.xml && \
		echo "</settings>" >> /root/.m2/settings.xml

RUN	mkdir -p ~/.config/rebar3/ && \
		echo '{plugins, [rebar3_hex]}.' > ~/.config/rebar3/rebar.config


RUN pip install awscli
