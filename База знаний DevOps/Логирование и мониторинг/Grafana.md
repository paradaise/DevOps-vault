Визуализатор логов и метрик, позволяет делать красивые и понятные dashbord с плитками, Сама ходит за артефактами к Elastic/Prometheus/Loki и так далее.



```yaml
apiVersion: 1

  

datasources:

- name: Prometheus

type: prometheus

access: proxy

url: http://prometheus:9090

isDefault: true

editable: true

  
  

- name: Loki

type: loki

access: proxy

url: http://loki:3100

editable: true
```
Пример конфига json, который создаст сразу 2 DataSource - prometheus и loki, в них Grafana будет ходить за артефактами