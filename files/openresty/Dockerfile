ARG LNMP_OPENRESTY_VERSION

FROM openresty/openresty:$LNMP_OPENRESTY_VERSION
MAINTAINER xiaqiancheng "qiancheng1019@foxmail.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# COPY ./apt/sources.list /etc/apt/sources.list
RUN mkdir -p /usr/local/openresty/nginx/cert
COPY ./cert /usr/local/openresty/nginx/cert

# RUN apt-get update
# RUN apt-get install -y snapd
# RUN systemctl enable snapd

# CMD [ "/sbin/init" ]
# /data/www
