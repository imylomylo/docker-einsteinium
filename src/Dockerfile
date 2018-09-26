# Dockerfile must start with a FROM instruction
# FROM instruction specifies the Base Image from which you are building
# FROM <image>[:<tag>]
FROM ubuntu:16.04
# Install bitcoind from PPA
RUN apt-get update && \
 apt-get install -y  build-essential pkg-config libc6-dev m4 g++-multilib autoconf libtool ncurses-dev unzip git python zlib1g-dev wget bsdmainutils automake libboost-all-dev libssl-dev libprotobuf-dev protobuf-compiler libqt4-dev libqrencode-dev libdb++-dev ntp ntpdate vim software-properties-common curl libcurl4-gnutls-dev cmake clang screen htop sudo ngrep libevent-dev

RUN gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4
RUN curl -o /usr/local/bin/gosu -SL "https://github.com/tianon/gosu/releases/download/1.4/gosu-$(dpkg --print-architecture)" \
    && curl -o /usr/local/bin/gosu.asc -SL "https://github.com/tianon/gosu/releases/download/1.4/gosu-$(dpkg --print-architecture).asc" \
    && gpg --verify /usr/local/bin/gosu.asc \
    && rm /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu

RUN git clone https://github.com/nanomsg/nanomsg && \
  cd nanomsg && \
  cmake . -DNN_TESTS=OFF -DNN_ENABLE_DOC=OFF && \
  make -j$(nproc) && \
  make install && \
  ldconfig  

ENV USER=emc2foundation
ENV REPO=einsteinium
ENV BRANCH=master

ADD https://api.github.com/repos/$USER/$REPO/git/refs/heads/$BRANCH version.json
RUN git clone -b$BRANCH https://github.com/$USER/$REPO.git 

RUN  cd ${REPO} && \
  EMC2_ROOT="/${REPO}" && \
  BDB_PREFIX="${EMC2_ROOT}/db4" && \
  mkdir -p $BDB_PREFIX && \
  wget 'http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz' && \
  echo '12edc0df75bf9abd7f82f821795bcee50f42cb2e5f76a6a281b85732798364ef db-4.8.30.NC.tar.gz' | sha256sum -c && \
  sleep 15 && \
  tar -xzvf db-4.8.30.NC.tar.gz && \
  cd db-4.8.30.NC/build_unix/ && \
  ../dist/configure -enable-cxx -disable-shared -with-pic -prefix=$BDB_PREFIX && \
  make -j$(nproc) && \
  make install && \
  cd ${EMC2_ROOT} && \
  ./autogen.sh && \
  ./configure LDFLAGS="-L${BDB_PREFIX}/lib/" CPPFLAGS="-I${BDB_PREFIX}/include/" -without-gui -without-miniupnpc --disable-tests --disable-bench --with-gui=no && \
  make -j$(nproc) 

RUN ln -sf /${REPO}/src/${REPO}d /usr/local/bin/${REPO}d && \
  ln -sf /${REPO}/src/${REPO}-cli /usr/local/bin/${REPO}-cli

RUN curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - && \
  apt-get install -y nodejs

    
# EXPOSE rpc port for the node to allow outside container access
EXPOSE 27771 27772 27773 27774 27775 27776 27777 27778

COPY entrypoint.sh /usr/local/bin/entrypoint.sh
COPY user /etc/sudoers.d/user
# There can only be one CMD instruction in a Dockerfile
# CMD provides defaults for an executing container
# Drop user into bash shell by default
CMD ["/usr/local/bin/entrypoint.sh"]

#docker run -it -p 27485:27485 verusd

#FROM ANOTHER TERMINAL
#curl --user someuser:somepass --data '{"method": "getblockchaininfo"}' http://127.0.0.1:27486
