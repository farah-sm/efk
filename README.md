```bash
# Setting up EFK Stack on Kubernetes

This guide outlines the steps to install and configure the EFK (Elasticsearch, Fluent Bit, Kibana) stack on a Kubernetes cluster.

## 1. Create a Namespace

kubectl create namespace logging
```

## 2. Add Helm Repositories

```bash
helm repo add elastic [https://helm.elastic.co](https://helm.elastic.co)
helm repo update
helm repo add fluent [https://fluent.github.io/helm-charts](https://fluent.github.io/helm-charts)
helm repo update
```

## 3. Install Elasticsearch

```bash
helm install elasticsearch \
  --set replicas=1 \
  --set persistence.labels.enabled=true \
  elastic/elasticsearch -n logging
```

## 4. Install Kibana

```bash
helm install kibana \
  --set service.type=LoadBalancer \
  elastic/kibana -n logging
```

## 5. Retrieve Elasticsearch Credentials

```bash
# for username
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.username}' | base64 -d

# for password
kubectl get secrets --namespace=logging elasticsearch-master-credentials -ojsonpath='{.data.password}' | base64 -d
```

**Important:** Store these credentials securely.

## 6. Configure Fluent Bit

*   Create a `fluentbit-values.yaml` file.
*   Locate the `HTTP_Passwd` field and replace the placeholder with the Elasticsearch password from the previous step.

```yaml
service:
  type: ClusterIP
  annotations: {}
env:
  # Place your Elasticsearch credentials here
  HTTP_User: "elastic"
  HTTP_Passwd: "your_elasticsearch_password" 
```

## 7. Install Fluent Bit

```bash
helm install fluent-bit \
  fluent/fluent-bit \
  -f fluentbit-values.yaml \
  -n logging
```

## 8. Access Kibana

*   Get the external IP of the Kibana LoadBalancer:

```bash
kubectl get service kibana -n logging
```

*   Open your browser and navigate to `http://<EXTERNAL-IP>:5601`.
*   Log in with the Elasticsearch credentials.

## 9. Clean Up

```bash
helm uninstall fluent-bit -n logging
helm uninstall elasticsearch -n logging
helm uninstall kibana -n logging

kubectl delete namespace logging
```

## Important Notes

*   This is a basic setup. For production, consider:
    *   Increased Elasticsearch replicas.
    *   Resource limits for pods.
    *   TLS encryption.
    *   Advanced log processing with Fluentd or Logstash.
*   Refer to official documentation for the latest information.
```
