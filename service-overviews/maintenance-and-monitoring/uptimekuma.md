# UptimeKuma

[Link to App](https://kuma-vps.xfgn.dev)

[Link to GitHub or Website](https://github.com/louislam/uptime-kuma)

Uptime Kuma is an easy-to-use self-hosted monitoring tool.

This app is hosted on Cola as a docker container

{% @github-files/github-code-block url="https://github.com/trentnbauer/agg/blob/main/docker-compose/uptimekuma.yml" %}

{% code title=".env" %}
```
CLOUDFLARE_APIKEY=REDACTED
DOMAIN=xfgn.dev
SUBDOMAIN=cola
PROXIED=false
PORT=3001
```
{% endcode %}

| Port | Purpose |
| ---- | ------- |
| 3001 | WebGUI  |

| Host Volume   | Container Volume | Purpose             |
| ------------- | ---------------- | ------------------- |
| docker volume | app/data         | app data and config |

| Integration | Purpose                               |
| ----------- | ------------------------------------- |
| Crowdsec    | Monitors logs using Docker connection |

\
