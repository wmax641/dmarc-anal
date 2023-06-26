FROM grafana/grafana-oss:10.0.1

ARG TZ=Australia/Sydney

USER root
RUN cp /usr/share/zoneinfo/${TZ} /etc/localtime

COPY --chown=grafana:root --chmod=755 grafana_dmarc_dashboard.json /var/lib/grafana/dashboards/
COPY --chown=grafana:root --chmod=755 grafana-provisioning/ /etc/grafana/provisioning/

USER grafana
