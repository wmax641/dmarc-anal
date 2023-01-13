FROM alpine:latest

ARG TZ=Australia/Sydney
ARG MMDB_VERSION

RUN apk update --no-cache --purge \
    && apk upgrade --no-cache
RUN apk add --no-cache python3 py3-pip tzdata \
    &&cp /usr/share/zoneinfo/Australia/Sydney /etc/localtime

WORKDIR /app

COPY parsedmarc.ini ./

# Comment this out if you don't have MaxMind GeoLite API credentials
# parsedmarc will then use the default geoip DB that it comes packaged with 
COPY GeoIP.conf ./
RUN wget https://github.com/maxmind/geoipupdate/releases/download/v${MMDB_VERSION}/geoipupdate_${MMDB_VERSION}_linux_amd64.tar.gz \
    && tar zxvf geoipupdate_${MMDB_VERSION}_linux_amd64.tar.gz \
    && mv geoipupdate_${MMDB_VERSION}_linux_amd64 geoipupdate \
    && /app/geoipupdate/geoipupdate -f /app/GeoIP.conf -d /app \
    && echo "1 2 * * 6 /app/geoipupdate/geoipupdate -f /app/GeoIP.conf -d /app" >> /etc/crontabs/root

RUN pip3 install parsedmarc

RUN adduser -D parsedmarc

USER parsedmarc

ENTRYPOINT ["parsedmarc", "-c", "parsedmarc.ini"]
#ENTRYPOINT ["tail", "-f", "/dev/null"]
