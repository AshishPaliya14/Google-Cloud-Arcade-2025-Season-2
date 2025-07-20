# Google-Cloud-Arcade-2025

# Deploy a static site with Caddy V2 on Google Cloud Run || Lab Solution || gem-cloud-run-caddy-website || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
#!/bin/bash
# Define color variables

BLACK=`tput setaf 0`
RED=`tput setaf 1`
GREEN=`tput setaf 2`
YELLOW=`tput setaf 3`
BLUE=`tput setaf 4`
MAGENTA=`tput setaf 5`
CYAN=`tput setaf 6`
WHITE=`tput setaf 7`

BG_BLACK=`tput setab 0`
BG_RED=`tput setab 1`
BG_GREEN=`tput setab 2`
BG_YELLOW=`tput setab 3`
BG_BLUE=`tput setab 4`
BG_MAGENTA=`tput setab 5`
BG_CYAN=`tput setab 6`
BG_WHITE=`tput setab 7`

BOLD=`tput bold`
RESET=`tput sgr0`
#----------------------------------------------------start--------------------------------------------------#

echo
echo -e "${CYAN}${BOLD_TEXT}==============================================${RESET_FORMAT}"
echo -e "${CYAN}${BOLD_TEXT}                LearnWithAshish               ${RESET_FORMAT}"
echo -e "${CYAN}${BOLD_TEXT}==============================================${RESET_FORMAT}"
echo
echo "${YELLOW}${BOLD}... Starting Execution ...${RESET}"
gcloud auth list

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

export PROJECT_ID=$(gcloud config get-value project)

gcloud config set compute/region "$REGION"
gcloud config set project "$PROJECT_ID"

gcloud services enable run.googleapis.com artifactregistry.googleapis.com cloudbuild.googleapis.com

gcloud artifacts repositories create caddy-repo --repository-format=docker --location="$REGION" --description="Docker repository for Caddy images"


cat > index.html <<EOF_CP
<html>
<head>
  <title>My Static Website</title>
</head>
<body>
  <div>Hello from Caddy on Cloud Run!</div>
  <p>This website is served by Caddy running in a Docker container on Google Cloud Run.</p>
</body>
</html>
EOF_CP

cat > Caddyfile <<EOF_CP
:8080
root * /usr/share/caddy
file_server
EOF_CP

cat > Dockerfile <<EOF_CP
FROM caddy:2-alpine

WORKDIR /usr/share/caddy

COPY index.html .
COPY Caddyfile /etc/caddy/Caddyfile
EOF_CP

docker build -t "$REGION"-docker.pkg.dev/"$PROJECT_ID"/caddy-repo/caddy-static:latest .

docker push "$REGION"-docker.pkg.dev/"$PROJECT_ID"/caddy-repo/caddy-static:latest

gcloud run deploy caddy-static --region=$REGION --image "$REGION"-docker.pkg.dev/"$PROJECT_ID"/caddy-repo/caddy-static:latest --platform managed --allow-unauthenticated

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
