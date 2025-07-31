# Google-Cloud-Arcade-2025

# Cloud Monitoring: Qwik Start || Lab Solution || GSP089 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

####   Export the ZONE Name correctly:

```
export ZONE=
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
gcloud config set project $DEVSHELL_PROJECT_ID
gcloud config set compute/zone $ZONE
export REGION="${ZONE%-*}"
gcloud config set compute/region $REGION

gcloud compute instances create lamp-1-vm --zone=$ZONE --project=$DEVSHELL_PROJECT_ID --machine-type=e2-medium --image-project=debian-cloud --image-family=debian-12 --tags=http-server
gcloud compute firewall-rules create allow-http --project=$DEVSHELL_PROJECT_ID --network=default --action=ALLOW --direction=INGRESS --priority=1000 --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

INSTANCE_CP=$(gcloud compute instances describe lamp-1-vm --zone=$ZONE --project=$DEVSHELL_PROJECT_ID --format='json' | jq -r '.id')

gcloud monitoring uptime create lamp-uptime-check --resource-type="gce-instance" --resource-labels=project_id=$DEVSHELL_PROJECT_ID,instance_id=$INSTANCE_CP,zone=$ZONE

# Create the email notification channel JSON
cat > email.json <<EOF_CP
{
  "type": "email",
  "displayName": "techcps",
  "description": "subscribe to techcps",
  "labels": {
    "email_address": "$USER_EMAIL"
  }
}
EOF_CP

# Create the notification channel
gcloud beta monitoring channels create --channel-content-from-file=email.json

# Get the channel info
channel_info=$(gcloud beta monitoring channels list)
channel_id=$(echo "$channel_info" | grep -oP 'name: \K[^ ]+' | head -n 1)

# Get the project ID
project_id=$(gcloud config get-value project)

# Create the JSON alert policy
cat > techcps-alert-policy.json <<EOF_CP
{
  "displayName": "Inbound Traffic Alert",
  "userLabels": {},
  "conditions": [
    {
      "displayName": "VM Instance - Network traffic",
      "conditionThreshold": {
        "filter": "resource.type = \"gce_instance\" AND metric.type = \"agent.googleapis.com/interface/traffic\"",
        "aggregations": [
          {
            "alignmentPeriod": "300s",
            "crossSeriesReducer": "REDUCE_NONE",
            "perSeriesAligner": "ALIGN_RATE"
          }
        ],
        "comparison": "COMPARISON_GT",
        "thresholdValue": 1000,
        "duration": "0s"
      }
    }
  ],
  "notificationChannels": [
    "$channel_id"
  ],
  "combiner": "OR",
  "enabled": true
}
EOF_CP

# Create the alert policy
gcloud alpha monitoring policies create --policy-from-file="techcps-alert-policy.json"
gcloud compute ssh lamp-1-vm --zone=$ZONE --project=$DEVSHELL_PROJECT_ID --quiet --command="sudo apt-get update && sudo apt-get install apache2 php7.0 -y && sudo service apache2 restart && curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh && sudo bash add-google-cloud-ops-agent-repo.sh --also-install -y && sudo systemctl status google-cloud-ops-agent"*" && sudo apt-get update"
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
