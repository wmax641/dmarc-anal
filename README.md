## dmarc-anal

PoC project based off @debricked/dmarc-visualizer, but using MS Graph to pull DMARC agg reports from an O365 mailbox.

Expected flow is to: 
* Read DMARC agg reports from an O365 inbox via MS Graph / Azure App
* Process DMARC agg reports, and output to ElasticSearch
* Grafana to read from ElasticSearch

## Setup
* Configure the `parsedmarc.ini` config file for MS Graph API usage, based off the sample provided at `./parsedmarc.ini.sample`. 
```
cd ./parsedmarc
cp parsedmarc.ini.sample parsedmarc.ini
```

* Choose target Elasticsearch and Grafana versions to use. Update references in:
```
./docker-compose.yml
./grafana/Dockerfile
./grafana/grafana_dmarc_dashboard.json
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
Grafana interface will be available on localhost:3000, and parsedmarc will begin reading from the configured mailbox/folder. However, the parsedmarc container may fail a few times before it works as it has to wait for elasticsearch app to become operatoinal.

