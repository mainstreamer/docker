FROM nginx:alpine

RUN apk add --update \
    acl \
    shadow \
    nano

RUN rm -rf /var/cache/apk/* && rm -rf /tmp/*

ADD .docker/nginx/nginx.conf /etc/nginx/
ADD .docker/nginx/host.conf /etc/nginx/conf.d/
ADD .docker/nginx/cert.crt /etc/ssl/cert.crt
ADD .docker/nginx/cert.key /etc/ssl/cert.key
RUN mkdir -p /var/www/webapp
ADD public /var/www/webapp/public

RUN rm /etc/nginx/conf.d/default.conf

RUN echo -e "upstream php-upstream { server php:9000; }" > /etc/nginx/conf.d/upstream.conf

RUN addgroup www-data

RUN adduser -D -g '' -G www-data www-data

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80
EXPOSE 443