# Wazuh

[Link to App](https://wazuh.xfgn.dev)

[Link to GitHub or Website](https://wazuh.com/)

Wazuh is an enterprise-ready platform used for security monitoring. It is a free and open-source platform that is used for threat detection, incident response and compliance, and integrity monitoring. Wazuh is capable of protecting workloads across virtualized, on-premises, containerized, and cloud-based environments.

### Wazuh Server

This app is installed directly on Lungo

<table><thead><tr><th width="375">Port</th><th>Purpose</th></tr></thead><tbody><tr><td>1514</td><td>Agent connection</td></tr><tr><td>1515</td><td>Agent enrollment</td></tr><tr><td>514</td><td>Syslog Collector</td></tr><tr><td>55000</td><td>Server RESTful API</td></tr><tr><td>9200</td><td>Indexer RESTful API</td></tr><tr><td>5601</td><td>Web UI</td></tr></tbody></table>

### Wazuh Agent

The agent is pushed via Ansible and installed directly on each host
