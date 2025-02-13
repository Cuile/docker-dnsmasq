FROM alpine:3.21
LABEL maintainer="dev@jpillora.com"
# setup dnsmasq and webproc binary
# COPY ../resources/webproc_0.4.0_linux_amd64.gz /etc/webproc_0.4.0_linux_amd64.gz
ENV WEBPROC_VERSION v0.4.0
ENV WEBPROC_URL https://github.com/jpillora/webproc/releases/download/webproc_$WEBPROC_VERSION_linux_amd64.gz
RUN apk update \
	&& apk --no-cache add dnsmasq \
	&& apk add --no-cache --virtual .build-deps curl \
	&& curl -sL $WEBPROC_URL | gzip -d - > /usr/local/bin/webproc \
	&& chmod +x /usr/local/bin/webproc \
	&& apk del .build-deps
# configure dnsmasq
RUN mkdir -p /etc/default/ \
	&& echo -e "ENABLED=1\nIGNORE_RESOLVCONF=yes" > /etc/default/dnsmasq
COPY ../conf/dnsmasq.conf /etc/dnsmasq.conf
COPY ../conf/dnsmasq.hosts /etc/ndsmasq.hosts
#run!
ENTRYPOINT ["webproc","-c","/etc/dnsmasq.conf","-c","/etc/dnsmasq.hosts","--","dnsmasq","--no-daemon"]
