FROM grafana/grafana-oss:9.3.2

ARG TZ=Australia/Sydney

USER root
RUN cp /usr/share/zoneinfo/Australia/Sydney /etc/localtime
USER grafana

COPY --chown=grafana:root grafana_dmarc_dashboard.json /var/lib/grafana/dashboards/
RUN chmod 744 /etc/grafana/provisioning

COPY grafana-provisioning/ /etc/grafana/provisioning/
