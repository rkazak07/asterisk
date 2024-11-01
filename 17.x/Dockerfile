FROM debian:stable-slim

ENV ASTERISK_VERSION=17.0.0

WORKDIR /src

COPY configs/ /etc/asterisk/

RUN apt-get update && \
    apt-get install -y git apt-utils curl file wget libnewt-dev libssl-dev libncurses5-dev libsqlite3-dev build-essential libjansson-dev libxml2-dev uuid-dev libedit-dev mpg123 ffmpeg subversion uuid-runtime \
    && export GNUPGHOME="$(mktemp -d)" \
  && wget https://downloads.asterisk.org/pub/telephony/asterisk/releases/asterisk-${ASTERISK_VERSION}.tar.gz \
  && tar xzf asterisk-${ASTERISK_VERSION}.tar.gz \
  && cd asterisk-${ASTERISK_VERSION} \
  && useradd --system asterisk \
  && ./configure --with-pjproject-bundled --with-jansson-bundled \
  && make menuselect.makeopts \
  && ./menuselect/menuselect \
    --disable BUILD_NATIVE \
    --enable chan_mobile \
    --disable chan_ooh323 \
    --enable format_mp3 \
    --disable res_config_mysql \
    --disable app_mysql \
    --disable cdr_mysql \
    --enable-category MENUSELECT_APPS \
    --disable app_skel \
    --disable app_fax \
    --disable app_ivrdemo \
    --disable app_saycounted \
    --disable app_statsd \
    --enable-category MENUSELECT_BRIDGES \
    --enable-category MENUSELECT_CDR \
    --disable chan_sip \
    --disable cdr_pgsql \
    --disable cdr_radius \
    --disable cdr_custom \
    --disable cdr_csv \
    --disable cdr_sqlite3_custom \
    --disable cdr_syslog \
    --enable-category MENUSELECT_CEL \
    --disable cel_pgsql \
    --disable cel_radius \
    --disable cel_custom \
    --disable chan_pjsip \
    --disable cel_manager \
    --disable cel_sqlite3_custom \
    --enable-category MENUSELECT_CHANNELS \
    --enable-category MENUSELECT_CODECS \
    --enable-category MENUSELECT_FORMATS \
    --enable-category MENUSELECT_FUNCS \
    --enable-category MENUSELECT_PBX \
    --enable pbx_lua \
    --enable-category MENUSELECT_RES \
    --disable res_mwi_external \
    --disable res_chan_stats \
    --disable res_endpoint_stats \
    --disable res_pktccops \
    --disable res_prometheus \
    --disable res_pjsip_transport_websocket \
    --enable-category MENUSELECT_TESTS \
    --enable-category MENUSELECT_UTILS \
    --disable aelparse \
    --disable astman \
    --disable check_expr \
    --disable check_expr2 \
    --disable conf2ael \
    --disable muted \
    --disable smsq \
    --disable stereorize \
    --enable  streamplayer \
    --disable astdb2sqlite3 \
    --disable astdb2bdb \
    --disable-category MENUSELECT_AGIS \
    --disable-category MENUSELECT_CORE_SOUNDS \
    --enable CORE-SOUNDS-EN-ULAW \
    --disable-category MENUSELECT_MOH \
    --enable MOH-OPSOUND-ULAW \
    --disable-category MENUSELECT_EXTRA_SOUNDS \
    --enable EXTRA-SOUNDS-EN-ULAW \
    menuselect.makeopts \
&& make -j $(nproc) \
&& sh contrib/scripts/get_mp3_source.sh \
&& make install \
&& devpackages=`dpkg -l|grep '\-dev'|awk '{print $2}'|xargs` \
&& DEBIAN_FRONTEND=noninteractive apt-get --yes purge --auto-remove \
  autoconf \
  build-essential \
  git \
  subversion \
  bzip2 \
  cpp \
  m4 \
  make \
  patch \
  perl \
  perl-modules \
  pkg-config \
  xz-utils \
  ${devpackages} \
&& rm -rf /var/lib/apt/lists/* \
&& mkdir -p /etc/asterisk/ \
         /var/spool/asterisk/fax \
&& chown -R asterisk:asterisk /etc/asterisk \
                           /var/*/asterisk \
                           /usr/*/asterisk \
&& chmod -R 750 /var/spool/asterisk \
&& cd .. \
&& rm * -R 


EXPOSE 5060/udp 4569/udp 10000-20000/udp
