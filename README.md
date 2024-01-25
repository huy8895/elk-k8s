- cai dat es 
```shell
kubectl create -f https://download.elastic.co/downloads/eck/1.8.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/1.8.0/operator.yaml
kubectl apply -f elasticsearch.yaml
```
- exec 
```shell
DEMO_SERVICE=demo-service-85bf9879d6-5mm6f
kubectl logs -f $DEMO_SERVICE -c filebeat-sidecar
kubectl exec --stdin --tty $DEMO_SERVICE   -- /bin/bash
kubectl logs -f $DEMO_SERVICE
kubectl exec --stdin --tty $DEMO_SERVICE   -c filebeat-sidecar -- /bin/bash
```
```
kubectl delete configmap kibana-kibana-helm-scripts \
kubectl delete serviceaccount post-delete-kibana-kibana \
kubectl delete serviceaccount pre-install-kibana-kibana \
kubectl delete jobs.batch post-delete-kibana-kibana \
kubectl delete configmap kibana-kibana-helm-scripts \
kubectl delete role post-delete-kibana-kibana \
kubectl delete role pre-install-kibana-kibana \
kubectl delete rolebinding post-delete-kibana-kibana

helm uninstall kibana
helm install kibana .
```
```
kubectl port-forward service/kb-svc 5601
kubectl port-forward service/demo-service 8895:8080
kubectl port-forward service/esservice 9200
```

Tạo mới user kibana_user
```shell
curl --location 'http://localhost:9200/_security/user/kibana_user' \
--header 'Content-Type: application/json' \
--user elastic:demo@123
--data-raw '{
    "password": "demo@123",
    "roles": [
        "kibana_system"
    ]
}'
```

nếu sử dụng elastic search cluster thì config: 
```yml
...
        env:
          - name: ELASTICSEARCH_HOSTS
            value: '["http://192.168.56.131:9200","http://192.168.56.133:9200","http://192.168.56.133:9200"]'
          - name: ELASTICSEARCH_USERNAME
            value: "kibana_system"
          - name: ELASTICSEARCH_PASSWORD
            value: "kibana@123"
        ports:
        - containerPort: 5601
```

file logstash config: 
```yml
    output {
      elasticsearch {
        hosts => ["http://192.168.56.131:9200","http://192.168.56.132:9200","http://192.168.56.133:9200"]
        user => "elastic"
        password => "121212"
      }
    }
```

filebeat config map:
```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: filebeat-configmap
data:
  filebeat.yml: |
    filebeat:
      config:
        modules:
          path: /usr/share/filebeat/modules.d/*.yml
          reload:
            enabled: true
      inputs:
        - type: log
          enabled: true
          paths:
            - /app/logs/*.log
    output.logstash:
      hosts: [ "192.168.56.132:5044" ]
```

- mount volume log 
khi chạy file 
```shell
kubectl apply -f demo-hostpath.yaml
```

thì logs sẽ được mount từ pod vào máy node. 
nếu chạy bằng minikube thì log sẽ cần exec vào trong minikube bằng lệnh: 
```shell
minikube ssh 
cd /var/log/demo-hostpath
```