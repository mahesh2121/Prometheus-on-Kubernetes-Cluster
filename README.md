How to Install Prometheus on Kubernetes Cluster?


ref prometheus-values.yaml for prometheus installation:

helm repo add prometheus-community \
https://prometheus-community.github.io/helm-charts


helm repo update

helm search repo kube-prometheus-stack --max-col-width 23


It might differ the version that you used helm install monitoring 
 
helm install monitoring \
prometheus-community/kube-prometheus-stack \
--values prometheus-values.yaml \
--version 16.10.0 \
--namespace monitoring \
--create-namespace

kubectl get all -n monitoring

kubectl port-forward \
svc/monitoring-kube-prometheus-prometheus 9090 \
-n monitoring

kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring

kubectl get cm kube-proxy -n kube-system -o yaml

watch -n 1 -t kubectl get pods -n kube-system

kubectl -n kube-system patch ds kube-proxy -p "{\"spec\":{\"template\":{\"metadata\":{\"labels\":{\"updateTime\":\"`date +'%s'`\"}}}}}"

------------------------------------------------

Deploy postgrs using helm chart..

helm repo add bitnami https://charts.bitnami.com/bitnami

helm repo update

helm search repo postgresql --max-col-width 23


use postgres-4.yaml values file for helm.

helm install postgres \
bitnami/postgresql \
--values postgres-values.yaml \
--version 12.8.4 \
--namespace db \
--create-namespace


kubectl get endpoints -n db


kubectl describe endpoints postgres-postgresql-metrics -n db


kubectl apply -f service-monitor.yaml

use Grafana dashboard for postgres 9628

