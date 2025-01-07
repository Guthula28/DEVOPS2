# Kubernetes Setup

## Step 1: Update System Packages

```
sudo apt update
sudo apt upgrade -y
sudo apt install -y curl wget
```

## Step 2: Install K3s Kubernetes Cluster

```
curl -sfL https://get.k3s.io | sh -
sudo systemctl status k3s
```

## Step 3: Configure Kubectl

```
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config && sudo chown $USER ~/.kube/config
sudo chmod 600 ~/.kube/config && export KUBECONFIG=~/.kube/config
```
* Execute the below commands

```
kubectl get nodes
kubectl cluster-info
```
# Helm Setup

```
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm -y
```
## Step 4: Testing (Optional)

```
kubectl create deployment  nginx-deployment --image nginx --replicas 2
kubectl get deployment nginx-deployment
kubectl get pods
```
* Now, expose this deployment, run

```
kubectl expose deployment nginx-deployment --type NodePort --port 80
kubectl get svc nginx-deployment
curl <<Your-Ubuntu-System-IP-Address>:<port>>
```


# Install using Helm

## Add helm repo

`helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`

## Update helm repo

`helm repo update`

## Install helm 

`helm install prometheus prometheus-community/prometheus`

## Expose Prometheus Service

This is required to access prometheus-server using your browser.

`kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-ext`


# Install using Helm

## Add helm repo

`helm repo add grafana https://grafana.github.io/helm-charts`

## Update helm repo

`helm repo update`

## Install helm 

`helm install grafana grafana/grafana`

## Expose Grafana Service

`kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-ext`

## To get secret

`kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`


