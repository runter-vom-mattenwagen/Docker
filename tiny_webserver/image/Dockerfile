FROM alpine

RUN apk add --update nginx && rm -rf /var/cache/apk/* && \
    mkdir /www-data

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80

CMD ["nginx"]
