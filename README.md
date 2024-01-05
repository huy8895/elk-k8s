- cai dat es 
```shell
kubectl create -f https://download.elastic.co/downloads/eck/1.8.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/1.8.0/operator.yaml
kubectl apply -f elasticsearch.yaml
```
- exec 
```shell
DEMO_SERVICE=demo-service-687f86f665-l8tvk 
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