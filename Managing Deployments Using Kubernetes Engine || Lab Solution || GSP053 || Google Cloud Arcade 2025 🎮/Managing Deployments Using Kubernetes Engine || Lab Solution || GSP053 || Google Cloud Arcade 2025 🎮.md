# Google-Cloud-Arcade-2025

# Managing Deployments Using Kubernetes Engine || Lab Solution || GSP053 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
Enter REGION=
```
```
#!/bin/bash

# Bright Foreground Colors
BLACK_TEXT=$'\033[0;90m'
RED_TEXT=$'\033[0;91m'
GREEN_TEXT=$'\033[0;92m'
YELLOW_TEXT=$'\033[0;93m'
BLUE_TEXT=$'\033[0;94m'
MAGENTA_TEXT=$'\033[0;95m'
CYAN_TEXT=$'\033[0;96m'
WHITE_TEXT=$'\033[0;97m'

NO_COLOR=$'\033[0m'
RESET_FORMAT=$'\033[0m'
BOLD_TEXT=$'\033[1m'
UNDERLINE_TEXT=$'\033[4m'

# Displaying start message
echo
echo "${CYAN_TEXT}${BOLD_TEXT}╔════════════════════════════════════════════════════════╗${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}|                      LearnWithAshish                   |${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}╚════════════════════════════════════════════════════════╝${RESET_FORMAT}"
echo
gcloud auth list

export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

export PROJECT_ID=$(gcloud config get-value project)

gcloud config set compute/zone "$ZONE"

gcloud config set compute/region "$REGION"

gsutil -m cp -r gs://spls/gsp053/orchestrate-with-kubernetes .

cd orchestrate-with-kubernetes/kubernetes

gcloud container clusters create bootcamp --machine-type e2-small --num-nodes 3 --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"
sed -i 's/image: "kelseyhightower\/auth:2.0.0"/image: "kelseyhightower\/auth:1.0.0"/' deployments/auth.yaml

kubectl create -f deployments/auth.yaml

kubectl get deployments

kubectl get pods

kubectl create -f services/auth.yaml

kubectl create -f deployments/hello.yaml
kubectl create -f services/hello.yaml

kubectl create secret generic tls-certs --from-file tls/
kubectl create configmap nginx-frontend-conf --from-file=nginx/frontend.conf
kubectl create -f deployments/frontend.yaml
kubectl create -f services/frontend.yaml

kubectl get services frontend

sleep 17

kubectl scale deployment hello --replicas=5

kubectl get pods | grep hello- | wc -l

kubectl scale deployment hello --replicas=3

kubectl get pods | grep hello- | wc -l

sed -i 's/image: "kelseyhightower\/auth:1.0.0"/image: "kelseyhightower\/auth:2.0.0"/' deployments/hello.yaml

kubectl get replicaset

kubectl rollout history deployment/hello

kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'

kubectl rollout resume deployment/hello

kubectl rollout status deployment/hello

kubectl rollout undo deployment/hello

kubectl rollout history deployment/hello

kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'

kubectl create -f deployments/hello-canary.yaml

kubectl get deployments

curl -ks https://`kubectl get svc frontend -o=jsonpath="{.status.loadBalancer.ingress[0].ip}"`/version
echo
echo -e "\e[41;97m🎉${WHITE}${BOLD} Congratulations for completing the Lab! 🎉 \e[0m"
echo
echo "📺 Subscribe to the channel: https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ"
echo

```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
