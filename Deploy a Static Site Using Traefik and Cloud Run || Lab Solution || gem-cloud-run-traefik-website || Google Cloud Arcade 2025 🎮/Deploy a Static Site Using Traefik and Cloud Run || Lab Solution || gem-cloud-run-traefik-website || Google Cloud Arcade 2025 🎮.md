# Google-Cloud-Arcade-2025

# Deploy a Static Site Using Traefik and Cloud Run || Lab Solution || gem-cloud-run-traefik-website || Google Cloud Arcade 2025 🎮

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

gcloud config set compute/region $REGION
gcloud config set project $PROJECT_ID

gcloud services enable run.googleapis.com artifactregistry.googleapis.com cloudbuild.googleapis.com

gcloud artifacts repositories create traefik-repo --repository-format=docker --location="$REGION" --description="Docker repository for static site images"

mkdir traefik-site && cd traefik-site && mkdir public

cat > public/index.html <<EOF
<html>
<head>
  <title>My Static Website</title>
</head>
<body>
  <p>Hello from my static website on Cloud Run!</p>
</body>
</html>
EOF

gcloud auth configure-docker "$REGION"-docker.pkg.dev

cat > traefik.yml <<EOF
entryPoints:
  web:
    address: ":8080"

providers:
  file:
    filename: /etc/traefik/dynamic.yml
    watch: true

log:
  level: INFO
EOF

cat > traefik.yml <<EOF
entryPoints:
  web:
    address: ":8080"

providers:
  file:
    filename: /etc/traefik/dynamic.yml
    watch: true

log:
  level: INFO
EOF

cat > dynamic.yml <<EOF
http:
  routers:
    static-files:
      rule: "PathPrefix(`/`)"
      entryPoints:
        - web
      service: static-service

  services:
    static-service:
      loadBalancer:
        servers:
          - url: "http://localhost:8000"
EOF

cat > Dockerfile <<EOF
FROM alpine:3.20

# Install traefik and caddy
RUN apk add --no-cache traefik caddy

# Copy configs and static files
COPY traefik.yml /etc/traefik/traefik.yml
COPY dynamic.yml /etc/traefik/dynamic.yml
COPY public/ /public/

# Cloud Run uses port 8080
EXPOSE 8080

# Run static server (on 8000) and Traefik (on 8080)
ENTRYPOINT [ "caddy" ]
CMD [ "file-server", "--listen", ":8000", "--root", "/public", "&", "traefik" ]
EOF

docker build -t "$REGION"-docker.pkg.dev/"$PROJECT_ID"/traefik-repo/traefik-static-site:latest .

docker push "$REGION"-docker.pkg.dev/"$PROJECT_ID"/traefik-repo/traefik-static-site:latest

gcloud run deploy traefik-static-site --region "$REGION" --image "$REGION"-docker.pkg.dev/"$PROJECT_ID"/traefik-repo/traefik-static-site:latest --platform managed --allow-unauthenticated --port 8000

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
