### filebeat配置

```yaml
setup.template.name: "learn-hacker"
setup.template.pattern: "learn-hacker-*"

filebeat.inputs:
  - type: kafka
    hosts:
      - localhost:9092
	    topics: ["logs"]
    group_id: "filebeat"
    parsers:
    - ndjson:
        keys_under_root: true

output.elasticsearch:
# 在kibana容器的data目录下
  ssl.certificate_authorities: ["/usr/share/filebeat/ca_1691726453972.crt"]
  username: "elastic"
  password: "012359clown"
  hosts: ["<https://localhost:9200>"]
  index: "learn-hacker-%{+yyyy.MM.dd}"
```
