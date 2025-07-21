# Google-Cloud-Arcade-2025

# Create a Go Artifact Registry and Upload Code || Lab Solution || gem-artifact-registry-go || Google Cloud Arcade 2025 🎮

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

gcloud services enable artifactregistry.googleapis.com

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

export PROJECT_ID=$(gcloud config get-value project)

gcloud config set compute/region $REGION
gcloud config set project $PROJECT_ID

gcloud artifacts repositories create my-go-repo \
    --repository-format=go \
    --location="$REGION" \
    --description="Go repository"

gcloud artifacts repositories describe my-go-repo \
    --location="$REGION"

go env -w GOPRIVATE=cloud.google.com/"$PROJECT_ID"

export GONOPROXY=github.com/GoogleCloudPlatform/artifact-registry-go-tools
GOPROXY=proxy.golang.org go run github.com/GoogleCloudPlatform/artifact-registry-go-tools/cmd/auth@latest add-locations --locations="$REGION"

mkdir hello
cd hello

go mod init labdemo.app/hello

cat > hello.go <<EOF
package main

import "fmt"

func main() {
	fmt.Println("Hello, Go module from Artifact Registry!")
}
EOF

go build

git config --global user.email "$EMAIL"

git config --global user.name cls 

git config --global init.defaultBranch main 

git init

git add .

git commit -m "Initial commit"

git tag v1.0.0

gcloud artifacts go upload \
  --repository=my-go-repo \
  --location="$REGION" \
  --module-path=labdemo.app/hello \
  --version=v1.0.0 \
  --source=.

gcloud artifacts packages list --repository=my-go-repo --location="$REGION"

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
