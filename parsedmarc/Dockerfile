FROM alpine:latest

ARG TZ=Australia/Sydney
ARG GEOIP_VER=5.1.1

RUN apk update --no-cache --purge \
    && apk upgrade --no-cache \
    && apk add --no-cache python3 py3-pip tzdata \
    && cp /usr/share/zoneinfo/${TZ} /etc/localtime

WORKDIR /app

COPY parsedmarc.ini ./

# If you have MaxMind Geoip API creds in ./GeoIP.conf, this will download the latest
# GeoIP database into the WORKDIR, and start a cronjob to update it once a week
# Comment these out if you don't have MaxMind GeoLite API credentials set up as described.
# parsedmarc will then instead use the default geoip DB that it comes packaged with 
COPY --chmod=640 GeoIP.conf ./
RUN wget https://github.com/maxmind/geoipupdate/releases/download/v${GEOIP_VER}/geoipupdate_${GEOIP_VER}_linux_amd64.tar.gz \
    && tar zxvf geoipupdate_${GEOIP_VER}_linux_amd64.tar.gz \
    && rm geoipupdate_${GEOIP_VER}_linux_amd64.tar.gz \
    && mv geoipupdate_${GEOIP_VER}_linux_amd64 geoipupdate \
    && /app/geoipupdate/geoipupdate -f /app/GeoIP.conf -d /app \
    && echo "1 2 * * 6 /app/geoipupdate/geoipupdate -f /app/GeoIP.conf -d /app" >> /etc/crontabs/root
# # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # # #

RUN pip3 install -r requirements.txt

RUN adduser -D parsedmarc

USER parsedmarc

ENTRYPOINT ["parsedmarc", "-c", "parsedmarc.ini"]
#ENTRYPOINT ["tail", "-f", "/dev/null"]
