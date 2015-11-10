FROM alpine-bash
MAINTAINER Julien Garcia Gonzalez <jgonzalez@wemanity.com>

COPY ./hugo_0.14_linux_amd64 /bin/hugo
ENV HUGO_THEME hyde
RUN mkdir /hugo
WORKDIR "/hugo"
# ENTRYPOINT exec /bin/hugo server -w --bind=hugo-server -t=$HUGO_THEME --baseUrl="http://192.168.99.101:1313" -s /hugo
ENTRYPOINT exec hugo server -b http://"hugo-server" -p "59000" /hugo

EXPOSE 1313
