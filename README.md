# Lab - Microsserviços na Prática

Veja o post no Blog -> [Microsserviços na Prática](https://finardi.me/microservicos-na-pratica/).

## Requisitos

1 - Git
2 - Docker
3 - Kubectl
4 - Kind

## Criando o cluster

```bash
git clone https://github.com/tfinardi/microservices-demo.git
cd microservices-demo
kind create cluster --config ./kind.yaml
```

## Deploy da aplicação

```bash
kubectl apply -f ./app/complete-demo.yaml
```

UI: http://localhost

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

*Espere até os pods do grafana ficarem disponíveis*

```bash
kubectl get pods -n monitoring --selector=app=grafana
NAME                            READY   STATUS    RESTARTS   AGE
grafana-core-589d98b9f4-7kr9g   1/1     Running   0          6m36s
```

## Importando Dashboards

```bash
kubectl apply -f 23-grafana-import-dash-batch.yaml
```

UI: http://localhost:3000
User/Pass: admin/admin

## Jaeger - trace

```
kubectl apply -f manifests-jaeger/
```

UI: http://localhost:8080

## Referências
* [Instalação do Docker](https://docs.docker.com/engine/install/ubuntu/)
* [Instalação do Kubectl](https://kubernetes.io/docs/tasks/tools/)
* [Insstalação do Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
* [Kind Configuration](https://kind.sigs.k8s.io/docs/user/configuration/)
* [Sock Shop microservices](https://github.com/microservices-demo)
* [Curso de Monitoração com o Prometheus - Rafael Cirolini](https://github.com/cirolini/prometheus-curso-monitoring)
