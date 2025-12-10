# Google-Cloud-Arcade-2025

# Detect and Investigate Threats with Security Command Center || Lab Solution || GSP1125 || Google Cloud Arcade 2025 🎮

## Subscribe : "Learn With Ashish" [![Subscribe on YouTube](https://img.shields.io/badge/-Subscribe%20on%20YouTube-FF0000?style=for-thebadge&logo=youtube&logoColor=white&labelColor=FF0000)](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)   


## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
#!/bin/bash

BLACK_TEXT=$'\033[0;90m'
RED_TEXT=$'\033[0;91m'
GREEN_TEXT=$'\033[0;92m'
YELLOW_TEXT=$'\033[0;93m'
BLUE_TEXT=$'\033[0;94m'
MAGENTA_TEXT=$'\033[0;95m'
CYAN_TEXT=$'\033[0;96m'
WHITE_TEXT=$'\033[0;97m'
DIM_TEXT=$'\033[2m'
STRIKETHROUGH_TEXT=$'\033[9m'
BOLD_TEXT=$'\033[1m'
RESET_FORMAT=$'\033[0m'

clear

echo
echo "${CYAN_TEXT}${BOLD_TEXT}===================================${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}            LearnWithAshish        ${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}===================================${RESET_FORMAT}"
echo

echo "${YELLOW_TEXT}${BOLD_TEXT}... Starting Execution ...${RESET_FORMAT}"
echo

gcloud auth list

gcloud services enable securitycenter.googleapis.com

export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

gcloud projects get-iam-policy $(gcloud config get-value project) \
    --format=json > policy.json

jq '{ 
  "auditConfigs": [ 
    { 
      "service": "cloudresourcemanager.googleapis.com", 
      "auditLogConfigs": [ 
        { 
          "logType": "ADMIN_READ" 
        } 
      ] 
    } 
  ] 
} + .' policy.json > updated_policy.json

gcloud projects set-iam-policy $(gcloud config get-value project) updated_policy.json

sleep 15

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
--member=user:demouser1@gmail.com --role=roles/bigquery.admin

gcloud projects remove-iam-policy-binding $DEVSHELL_PROJECT_ID \
--member=user:demouser1@gmail.com --role=roles/bigquery.admin

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
  --member=user:$USER_EMAIL \
  --role=roles/cloudresourcemanager.projectIamAdmin 2>/dev/null

gcloud compute instances create instance-1 \
--zone=$ZONE --project=$DEVSHELL_PROJECT_ID \
--machine-type=e2-medium \
--network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default \
--metadata=enable-oslogin=true --maintenance-policy=MIGRATE --provisioning-model=STANDARD \
--scopes=https://www.googleapis.com/auth/cloud-platform --create-disk=auto-delete=yes,boot=yes,device-name=instance-1,image=projects/debian-cloud/global/images/debian-11-bullseye-v20230912,mode=rw,size=10,type=projects/$DEVSHELL_PROJECT_ID/zones/$ZONE/diskTypes/pd-balanced

gcloud dns policies create dns-test-policy --project=$DEVSHELL_PROJECT_ID --description="new policy" --networks="default" --private-alternative-name-servers="" --no-enable-inbound-forwarding --enable-logging

sleep 45

gcloud compute ssh instance-1 --zone=$ZONE --tunnel-through-iap --project "$DEVSHELL_PROJECT_ID" --quiet --command "gcloud projects get-iam-policy \$(gcloud config get project) && curl etd-malware-trigger.goog"

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

gcloud compute instances delete instance-1 --zone=$ZONE --quiet

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
