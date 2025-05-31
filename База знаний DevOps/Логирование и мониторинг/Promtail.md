**Что это?**  
Агент для **сбора логов** и отправки в **Loki**.

**Как работает?**

1. Читает логи из файлов (`/var/log/nginx/error.log`).
    
2. Добавляет метки (например, `app: nginx`).
    
3. Отправляет в Loki.
    

**Зачем?**

- Легковесная альтернатива Filebeat/Fluentd.
    
- Оптимизирован для работы с Loki.



Пример конфига для docker-compose

```yaml
server:
  http_listen_port: 9080 #Порт веб-интерфейса(promtail будет его слушать)
  grpc_listen_port: 0 #Отключаем gRPS

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://loki:3100/loki/api/v1/push #путь куда будут отправлятся логи в Loki

#собирает логи всех контейнеров из стандарной папки docker с добавлением метки job: docker-logs для фильтрации в Loki
scrape_configs:
- job_name: docker-logs
  static_configs:
  - targets:
      - localhost
    labels:
      job: docker-logs
      __path__: /var/lib/docker/containers/*/*log

  pipeline_stages:
  - json: #Разибрает JSON-логи Docker
      expressions:
        stream: stream #stdout/stderr
        attrs: attrs #атрибуты лога
        tag: attrs.tag #тег, например имя контейнера
        
  - regex: #разбирает тег на отдельные поля
      expression: (?P<image_name>(?:[^|]*[^|])).(?P<container_name>(?:[^|]*[^|])).(?P<image_id>(?:[^|]*[^|])).(?P<container_id>(?:[^|]*[^|]))
      source: "tag"

  - labels: #добавляет поля как метки в Loki
      tag:
      image_name: 
      container_name:
```

