FROM	alpine:3.10
LABEL   maintainer="rgerhards@adiscon.com"
COPY	rsyslog@lists.adiscon.com-5a55e598.rsa.pub /etc/apk/keys/rsyslog@lists.adiscon.com-5a55e598.rsa.pub
RUN	echo "http://alpine.rsyslog.com/3.10/stable" >> /etc/apk/repositories
RUN	echo "http://dl-cdn.alpinelinux.org/alpine/v3.10/main" >> /etc/apk/repositories
RUN	apk --no-cache --allow-untrusted update 
RUN	apk fix --upgrade --allow-untrusted alpine-keys
RUN	apk add --no-cache --allow-untrusted \
	   rsyslog \
	   rsyslog-elasticsearch \
	   rsyslog-imptcp \
	   rsyslog-imrelp \
	   rsyslog-mmjsonparse \
	   rsyslog-mmutf8fix \
	   rsyslog-omrelp \
	   rsyslog-omstdout \
	   logrotate
# Added logrotate to see if that helps
RUN	adduser -s /bin/ash -D rsyslog rsyslog \
	&& echo "rsyslog ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
VOLUME	/config /work /logs
CMD	["rsyslog"]
ENTRYPOINT ["/home/appliance/starter.sh"]
COPY	rsyslog.conf /etc/rsyslog.conf
COPY	rsyslog.conf.d/*.conf /etc/rsyslog.conf.d/
# done base system setup

WORKDIR /home/appliance
COPY	starter.sh CONTAINER.* ./
COPY	internal/* ./internal/
COPY	tools/* ./tools/
RUN	echo "`date +%F` (`date +%s`)" > CONTAINER.release \
	&& chown -R rsyslog:rsyslog *
