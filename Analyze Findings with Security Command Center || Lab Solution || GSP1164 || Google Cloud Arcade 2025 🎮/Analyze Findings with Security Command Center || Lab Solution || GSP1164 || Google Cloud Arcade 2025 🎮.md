# Google-Cloud-Arcade-2025

# Analyze Findings with Security Command Center || Lab Solution || GSP1164 || Google Cloud Arcade 2025 🎮

## Subscribe : "Learn With Ashish" [![Subscribe on YouTube](https://img.shields.io/badge/-Subscribe%20on%20YouTube-FF0000?style=for-thebadge&logo=youtube&logoColor=white&labelColor=FF0000)](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)   


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

# Array of color codes excluding black and white
TEXT_COLORS=($RED $GREEN $YELLOW $BLUE $MAGENTA $CYAN)
BG_COLORS=($BG_RED $BG_GREEN $BG_YELLOW $BG_BLUE $BG_MAGENTA $BG_CYAN)

# Pick random colors
RANDOM_TEXT_COLOR=${TEXT_COLORS[$RANDOM % ${#TEXT_COLORS[@]}]}
RANDOM_BG_COLOR=${BG_COLORS[$RANDOM % ${#BG_COLORS[@]}]}
echo
echo -e "${CYAN}${BOLD_TEXT}==============================================${RESET_FORMAT}"
echo -e "${CYAN}${BOLD_TEXT}                LearnWithAshish               ${RESET_FORMAT}"
echo -e "${CYAN}${BOLD_TEXT}==============================================${RESET_FORMAT}"
echo

gcloud auth list

export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

export PROJECT_ID=$(gcloud config get-value project)

gcloud config set compute/zone "$ZONE"

gcloud config set compute/region "$REGION"

gcloud services enable securitycenter.googleapis.com --quiet

export BUCKET_NAME="scc-export-bucket-$PROJECT_ID"

gcloud pubsub topics create projects/$DEVSHELL_PROJECT_ID/topics/export-findings-pubsub-topic

gcloud pubsub subscriptions create export-findings-pubsub-topic-sub \
  --topic=projects/$DEVSHELL_PROJECT_ID/topics/export-findings-pubsub-topic

echo
echo -e "\033[1;33mCreate an export-findings-pubsub\033[0m \033[1;34mhttps://console.cloud.google.com/security/command-center/config/continuous-exports/pubsub?project=$DEVSHELL_PROJECT_ID\033[0m"
echo

while true; do
    echo -ne "\e[1;93mDo you Want to proceed? (Y/n): \e[0m"
    read confirm
    case "$confirm" in
        [Yy]) 
            echo -e "\e[34mRunning the command...\e[0m"
            break
            ;;
        [Nn]|"") 
            echo "Operation canceled."
            break
            ;;
        *) 
            echo -e "\e[31mInvalid input. Please enter Y or N.\e[0m" 
            ;;
    esac
done

gcloud compute instances create instance-1 --zone=$ZONE \
  --machine-type=e2-micro \
  --scopes=https://www.googleapis.com/auth/cloud-platform

bq --location=$REGION mk --dataset $PROJECT_ID:continuous_export_dataset

gcloud scc bqexports create scc-bq-cont-export \
  --dataset=projects/$PROJECT_ID/datasets/continuous_export_dataset \
  --project=$PROJECT_ID \
  --quiet

for i in {0..2}; do
gcloud iam service-accounts create sccp-test-sa-$i;
gcloud iam service-accounts keys create /tmp/sa-key-$i.json \
--iam-account=sccp-test-sa-$i@$PROJECT_ID.iam.gserviceaccount.com;
done

query_findings() {
  bq query --apilog=/dev/null --use_legacy_sql=false --format=pretty \
    "SELECT finding_id, event_time, finding.category FROM continuous_export_dataset.findings"
}

has_findings() {
  echo "$1" | grep -qE '^[|] [a-f0-9]{32} '
}
wait_for_findings() {
  while true; do
    result=$(query_findings)
    
    if has_findings "$result"; then
      echo "Findings detected!"
      echo "$result"
      break
    else
      echo "No findings yet. Waiting for 100 seconds..."
      sleep 100
    fi
  done
}
wait_for_findings

gsutil mb -l $REGION gs://$BUCKET_NAME/
gsutil pap set enforced gs://$BUCKET_NAME

sleep 20

gcloud scc findings list "projects/$PROJECT_ID" \
  --format=json | jq -c '.[]' > findings.jsonl

sleep 20

gsutil cp findings.jsonl gs://$BUCKET_NAME/

echo
echo -e "\033[1;33mCreate an old_findings\033[0m \033[1;34mhttps://console.cloud.google.com/bigquery?project=$DEVSHELL_PROJECT_ID\033[0m"
echo

```

* Paste in the following schema:

```
[   
  {
    "mode": "NULLABLE",
    "name": "resource",
    "type": "JSON"
  },   
  {
    "mode": "NULLABLE",
    "name": "finding",
    "type": "JSON"
  }
]

```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
