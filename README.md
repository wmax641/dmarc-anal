## dmarc-anal

DMARC analysis visualiser

PoC based off @debricked/dmarc-visualizer, but using MS Graph to pull DMARC agg reports from an O365 mailbox, and with updated dashboards.

Expected flow is to: 
* Read DMARC agg reports from an O365 inbox via MS Graph / Azure App
* Process DMARC agg reports, and output to ElasticSearch
* Grafana to read from ElasticSearch

## Setup
* Configure the `./parsedmarc/parsedmarc.ini` config file for MS Graph API usage, based off the sample provided at `./parsedmarc.ini.sample`. Also edit any other configurations like mailbox/folder name.
```
cd ./parsedmarc
cp parsedmarc.ini.sample parsedmarc.ini
# (edit parsedmarc.ini)
```

* Set up Maxmind GeoLite2 API credentials; `./parsedmarc/GeoIP.conf`. If you don't have your own MaxMind GeoLite2 API credentials, then comment out GeoIP DB config in the below files. `parsedmarc` will then use the default DB that it comes packaged with, which is a bit old.
```
./parsedmarc/GeoIP.conf         # fill with Maxmind GeoLite2 API credentials
./parsedmarc/parsedmarc.ini
```

* Update timezones in dockerfiles;
```
./grafana/Dockerfile
./parsedmarc/Dockerfile
```

## Run
```
docker-compose up -d
```
Grafana interface will be available on localhost:3000, and parsedmarc will begin reading from the configured mailbox/folder. However, the parsedmarc container may fail a few times before it works as it waits for elasticsearch app to become operatoinal.

Then send some DMARC aggregate reports as attachments into the targeted mailbox/folder

## Notes

* Currently have to use elasticsearch 7.x, as 8.x operates radically differently with certs and auth.

* The only part of this project which reads untrusted input is the `parsedmarc` package reading XML DMARC reports sent in from the internet. `parsedmarc` itself uses the `xmltodict` package to do this risky operation. `xmltodict` itself is using the special `defusedxml / defusedexpat` XML modules which uses modified subclasses of stdlib XML parsers to prevent potentially malicious operation.

see - https://docs.python.org/3/library/xml.html#defusedxml-package
