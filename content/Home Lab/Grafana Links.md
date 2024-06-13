---
dg-publish: true
---


## Get Grafana installed with `docker-compose.yaml`


```yaml
version: '3.8'
services:
  grafana:
    image: grafana/grafana-enterprise
    container_name: grafana
    restart: unless-stopped
    ports:
      - '3000:3000'
    volumes:
      - grafana_data:/var/lib/grafana
volumes:
  grafana_data: {}
```



## Links for later:

https://grafana.com/docs/grafana/latest/alerting/
https://grafana.com/docs/grafana/latest/panels-visualizations/
https://grafana.com/docs/grafana/latest/dashboards/
https://grafana.com/docs/grafana/latest/setup-grafana/
https://grafana.com/docs/grafana/latest/datasources/
https://grafana.com/docs/grafana/latest/fundamentals/

## UniFi Networking

[Non-Unifi switch on Unifi network : r/Ubiquiti](https://www.reddit.com/r/Ubiquiti/comments/9qdo8l/nonunifi_switch_on_unifi_network/)
