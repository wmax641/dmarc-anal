## dmarc-anal

PoC project based off @debricked/dmarc-visualizer, but using MS Graph to pull DMARC agg reports from an O365 mailbox, and also built with production usage in mind

Expected flow is to: 
* Read DMARC agg reports from an O365 inbox via MS Graph / Azure App
* Process DMARC agg reports, and output to ElasticSearch
* Grafana to read from ElasticSearch

## Setup
* Configure the `parsedmarc.ini` config file for MS Graph API usage, based off the sample. 
```
cd parsedmarc
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
