##  Installation of Prometheus
* [Refer Here](https://prometheus.io/download/) for official docs

* Steps to install:

```
wget https://github.com/prometheus/prometheus/releases/download/v2.49.1/prometheus-2.49.1.linux-amd64.tar.gz
```
* untar the file

```
tar xvzf prometheus-2.49.1.linux-amd64.tar.gz
```
* go inside the directory

```
cd prometheus-2.49.1.linux-amd64
```
* Start the prometheus server with the command ./prometheus
* If you want to run in the background use ``` nohup ./prometheus & ``` after excuting just enter command two times
* The Prometheus server should start on port ``` 9090 ```
* You can access the Prometheus graph UI by visint ``` http://localhost:9090/graph ```
* You can access the Prometheus metrics UI by visint ``` http://localhost:9090/metrics ```

# How to create and Install Node exporter
* Node exporter is responsible for fetching the statistics from various hardware and virtual resources in the format which Prometheus can understand and with the help of the prometheus server those statistics can be exposed on port 9100.

* There are many third-party Node exporters which can be used by SREs as well as DevOps based on their application needs. But primarily we look for the following metrics -

  * CPU usage
  * Memory usage
  * Disk usage
  * Network usage

## How to Install Node exporter
* After installing the Prometheus in the previous step the next package we are going to install is **Node Exporter **. Node exported is used for collecting various hardware and kernel-level metrics of your machine.
* Download the binary of Node exporter based on the operating system. [Refer Here](https://prometheus.io/download/#node_exporter)

```
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
```
* Extract the download node exporter binary file.

```
tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz
```
* Go into the extracted directory

```
  cd node_exporter-1.7.0.linux-amd64
```
* Start the node exported with the command ./node_exporter
* If you want to run in the background use ``` nohup ./node_exporter & ``` after excuting just enter command two times
* Access the Node exporter metrics on the browser with URL - ``` http://localhost:9100 ```

### Adding node exporter scrap_configs to prometheus as a YAML Configuration
* In the previous node export installation, we had Prometheus and node exporter running on the same server. But if you want to add node exporter for any remote server then you need to define scrap_configs with target hosts inside the YAML configuration.
* Here is an example of scrap_configs -

```

global:
   scrape_interval: 15s

scrape_configs:
   - job_name: node
     static_configs:
        - targets: ['localhost:9100','100.0.0.3:9100'] 
```

* Start the prometeus

```

./prometheus --config.file=exporter.yml

```
* If you want to run in the background use ``` nohup ./prometheus --config.file=exporter.yml & ``` after excuting just enter command two times
* After adding the remote host into the node exporter configuration verify it by accessing the Prometheus target URL ``` localhost:9090/targets ```

# Installing Grafana
* In this installation, we are going to install the latest enterprise edition Grafana.
* Update the package info -

```
sudo apt-get install -y apt-transport-https
sudo apt-get install -y software-properties-common wget
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

* Add stable repository of Grafana -

```

echo "deb https://packages.grafana.com/enterprise/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
* Update repository and Install Grafana

```
sudo apt-get update
sudo apt-get install grafana
```
* Start the Grafana Server -

```
sudo systemctl daemon-reload
sudo systemctl start grafana-server
sudo systemctl status grafana-server
```
* Configure Grafana to run at boot time-

```

sudo systemctl enable grafana-server.service
 
```
* Access the Grafana Dashboard using ``` http://localhost:3000/login ```

* The default login Username: Password for accessing the Grafana dashboard is admin: admin
* Change the default admin password after the login

# Setting up the Grafana Dashboard
* The next step after installing the Grafana Server would be to set up the Grafana Dashboard. Till now, we have installed the Prometheus Server, Node Exporter as well as Grafana Server but there is a connection between Prometheus and Grafana.

* As I mentioned earlier Grafana is an analytics and visualization dashboard, Grafana does not store data, but instead it relies on Datasource connection towards the Prometheus server because Prometheus server is responsible for scraping and storing the data.

* Let's create a Prometheus Datasource inside Grafana Dashboard
Goto your Grafana Dashboard and click on Gear(Settings Icon)->Data Sources

* Click on Add Data Source

* Select the Prometheus as preferred data source -

* Enter the hostname or IP address of the prometheus server

* Save and test data source

#  Import Grafana Dashboard from Grafana Labs
* [Refer Here](https://grafana.com/grafana/dashboards/) for grafana labs
* Goto Grafana Dashboard and search for Linux Memory
* Click on the result and then copy the board ID .e.g 2747
* Goto Import and enter the board ID .e.g. 2747
* Click load and then select the data source as Prometheus
* Finally Click on Import
* You should be able to see the memory dashboard

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



