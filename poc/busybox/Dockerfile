## https://github.com/docker-library/busybox/tree/dist-amd64/latest/glibc
## docker build -t my_busybox .
## docker container rm -f busybox_container && docker run -it --name busybox_container my_busybox
FROM scratch
ADD busybox.tar.xz /
CMD ["sh"]