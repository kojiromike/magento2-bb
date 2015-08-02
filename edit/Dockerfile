FROM alpine
MAINTAINER Michael A. Smith <michael@smith-li.com>
RUN apk --update add vim \
 && adduser -D editor
WORKDIR /home/editor
COPY vimrunner /usr/local/bin/vimrunner
ENTRYPOINT ["/usr/local/bin/vimrunner"]
