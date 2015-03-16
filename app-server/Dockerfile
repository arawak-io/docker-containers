FROM debian:wheezy

MAINTAINER Lovell Felix <lovellfelix@gmail.com>

RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 7937DFD2AB06298B2293C3187D33FF9D0246406D 114F43EE0176B71C7BC219DD50A3051F888C628D

ENV NODE_VERSION 0.12.0
ENV NPM_VERSION 2.5.1
ENV APP_URL http://example.com
ENV APP_COMPANY_NAME MyCompany
ENV APP_MONGO_DB_HOST 127.0.0.1
ENV APP_MONGO_DB_USERNAME dbuser
ENV APP_MONGO_DB_PASSWORD dbpass
ENV APP_MONGO_DB_PORT 12707
ENV APP_LICENSE 7937DFD2AB06298B2293C3187D33FF9D0246406D
ENV APP_DATA_DIR /home/data
ENV APP_TMP_DIR /tmp
ENV APP_LOG_DIR /tmp

RUN buildDeps='curl' \
	&& set -x \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends openssh-server git-core bzip2 \
	&& rm -rf /var/lib/apt/lists/* \
	&& curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
	&& curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
	&& gpg --verify SHASUMS256.txt.asc \
	&& grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt.asc | sha256sum -c - \
	&& tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc \
	&& apt-get purge -y --auto-remove $buildDeps \
	&& npm install -g npm@"$NPM_VERSION" \
	&& npm install -g bower grunt-cli mean-cli gulp \
	&& npm cache clear

RUN mkdir /opt/cashew

WORKDIR /opt/cashew

RUN git config --global http.sslVerify false \
  && git clone -q https://github.com/arawak-io/webapp.git .

VOLUME ["/opt/app"]

RUN npm install


CMD ["gulp"]

EXPOSE 3000:3000
