
FROM alpine:3.18
LABEL Maintainer="atomspring"

RUN set -x && apk add --no-cache bash libgcc libstdc++ curl curl-dev coreutils ca-certificates tzdata shadow libstdc++ logrotate py3-pip s6-overlay nodejs-current yarn\
  && groupadd --system --gid 10000 node_app \
  && useradd --system --gid node_app --no-create-home --home /nonexistent --comment "Node App User Acct." --shell /bin/false --uid 10000  node_app\
  && mkdir -p /app /config /defaults \
  && pip3 install tzupdate \
  && apk del --purge \
  && rm -rf /tmp/* \
  && sed -i "s#/var/log/messages {}.*# #g" /etc/logrotate.conf \
  && node --version && npm --version && yarn --version


WORKDIR /app
RUN git clone git://github.com/nightscout/cgm-remote-monitor.git /app
RUN cd /app &&  git checkout ${DEPLOY_HEAD-master} && npm install --cache /tmp/empty-cache && npm run postinstall && npm run env && rm -rf /tmp/* \.

EXPOSE 1001

COPY rootfs /

USER node_app

#CMD ["node", "server.js"]
ENTRYPOINT [ "/init" ]