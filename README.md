# Microservices Demo Lab

See the [Blog post](#).

## Requirements

1 - Git
2 - Docker
3 - Kubectl
4 - Kind

## Creating the cluster

```bash
git clone https://github.com/tfinardi/microservices-demo.git
cd microservices-demo
kind create cluster --config ./kind.yaml
```


## Deploy Sock-Shop

```bash
kubectl apply -f complete-demo.yaml
```

## Deploy do Prometheus, kube-state-metrics e node-exporter

```bash
cd manifests-monitoring
kubectl apply -f 00-monitoring-ns.yaml
```

```bash
kubectl apply $(ls *-prometheus-*.yaml | awk ' { print " -f " $1 } ')
```

```bash
kubectl apply $(ls *-prometheus-*.yaml | awk ' { print " -f " $1 } ')
```

UI: http://localhost:9090

## deploy grafana

```bash
kubectl apply $(ls 2[0-2]-grafana-*.yaml | awk ' { print " -f " $1 } ')
```

*wait for the  Grafana POD to go up*

```bash
kubectl get pods -n monitoring --selector=app=grafana
NAME                            READY   STATUS    RESTARTS   AGE
grafana-core-589d98b9f4-7kr9g   1/1     Running   0          6m36s
```

## import dashboards
```bash
kubectl apply -f 23-grafana-import-dash-batch.yaml
```

## Jaeger - trace
To do
