FROM ubuntu:24.04
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && \
    apt-get install -y apache2 && \
    apt-get clean
RUN sed -i 's/Listen 80/Listen 6400/' /etc/apache2/ports.conf && \
    sed -i 's/<VirtualHost \*:80>/<VirtualHost *:6400>/' /etc/apache2/sites-available/000-default.conf

EXPOSE 6400
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]