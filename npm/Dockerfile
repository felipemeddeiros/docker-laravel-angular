FROM node:10.20

WORKDIR /var/www/webv2

RUN apt-get update \
  && npm install -g @angular/cli@9.1.4 \
  && rm -rf /tmp/* /var/cache/apk/* *.tar.gz ~/.npm \
  && npm cache clear --force \
  && yarn cache clean \
  && sed -i -e "s/bin\/ash/bin\/sh/" /etc/passwd

USER node

EXPOSE 4200
