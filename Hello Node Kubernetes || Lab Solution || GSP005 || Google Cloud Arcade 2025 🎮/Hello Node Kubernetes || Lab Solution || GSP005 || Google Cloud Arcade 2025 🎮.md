# Google-Cloud-Arcade-2025

# Hello Node Kubernetes || Lab Solution || GSP005 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

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

cat > server.js <<EOF_CP
var http = require('http');
var handleRequest = function(request, response) {
  response.writeHead(200);
  response.end("Hello World!");
}
var www = http.createServer(handleRequest);
www.listen(8080);
EOF_CP

cat > Dockerfile <<EOF_CP
FROM node:6.9.2
EXPOSE 8080
COPY server.js .
CMD node server.js
EOF_CP

docker build -t gcr.io/$PROJECT_ID/hello-node:v1 .

docker run -d -p 8080:8080 gcr.io/$PROJECT_ID/hello-node:v1

curl http://localhost:8080

ID=$(docker ps --format '{{.ID}}')

docker stop $ID

gcloud auth configure-docker --quiet

docker push gcr.io/$PROJECT_ID/hello-node:v1

gcloud config set project $PROJECT_ID

gcloud container clusters create hello-world --zone="$ZONE" --num-nodes 2 --machine-type n1-standard-1

kubectl create deployment hello-node --image=gcr.io/$PROJECT_ID/hello-node:v1

sleep 5

kubectl get deployments

sleep 5

kubectl get pods

kubectl cluster-info

kubectl config view

kubectl get events

kubectl expose deployment hello-node --type="LoadBalancer" --port=8080
sleep 7
kubectl get services
kubectl scale deployment hello-node --replicas=4
sleep 5
kubectl get deployment
sleep 7
kubectl get pods
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
