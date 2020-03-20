FROM haproxy:alpine

LABEL maintainer="mggggk<mggggk@gmail.com>"

RUN set -x	\
	&& apk upgrade --update \
	&& apk add bash ca-certificates rsyslog \
	&& mkdir -p /etc/rsyslog.d/ \
	&& touch /var/log/haproxy.log \
  && : "Generate stdout link for log." \
	&& ln -sf /dev/stdout /var/log/haproxy.log \
  && : "Generate log directory" \
	&& mkdir -p /share/log

RUN : "Generate rsyslog config" && { \
    echo '$ModLoad imudp' ; \
    echo ; \
    echo '$UDPServerAddress 0.0.0.0' ; \
    echo '' ; \
    echo '$UDPServerRun 514' ; \
    echo ; \
    echo 'local0.* /var/log/haproxy.log' ; \
    echo 'local0.* /share/log/haproxy.log' ; \
    echo ; \
    echo '& ~' ; \
    echo ; \
  } | tee /etc/rsyslog.conf

RUN : "Generate entrypoint.sh" && { \
    echo '#!/bin/bash' ; \
    echo ; \
    echo 'set -o errexit' ; \
    echo 'set -o nounset' ; \
    echo ; \
    echo '# Generate log file.' ; \
    echo 'mkdir -p /share/log' ; \
    echo 'touch /share/log/haproxy.log' ; \
    echo ; \
    echo '# Launch rsyslog and haproxy.' ; \
    echo 'readonly RSYSLOG_PID="/var/run/rsyslogd.pid"' ; \
    echo 'rm -f $RSYSLOG_PID' ; \
    echo 'rsyslogd' ; \
    echo 'exec haproxy "$@"' ; \
    echo ; \
  } | tee /entrypoint.sh \
    && chmod +x /entrypoint.sh

ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "-f", "/etc/haproxy.cfg" ]

