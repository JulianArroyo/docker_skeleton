## docker build -t my_php_alpine_3-18 .
## docker container rm -f php_alpine_3-18_container && docker run -it --name php_alpine_3-18_container my_php_alpine_3-18
FROM my_alpine

RUN apk update && \
    apk upgrade && \
    apk add php php-fpm && \
    apk add curl

CMD ["/bin/sh"]