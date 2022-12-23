FROM alpine:latest

RUN apk update --no-cache --purge
RUN apk upgrade --no-cache
RUN apk add --no-cache python3 py3-pip tzdata
ENV TZ=Australia/Sydney
RUN cp /usr/share/zoneinfo/Australia/Sydney /etc/localtime

WORKDIR /app

COPY parsedmarc.ini ./

RUN pip3 install parsedmarc

ENTRYPOINT ["parsedmarc", "-c", "parsedmarc.ini"]
#ENTRYPOINT ["tail", "-f", "/dev/null"]
