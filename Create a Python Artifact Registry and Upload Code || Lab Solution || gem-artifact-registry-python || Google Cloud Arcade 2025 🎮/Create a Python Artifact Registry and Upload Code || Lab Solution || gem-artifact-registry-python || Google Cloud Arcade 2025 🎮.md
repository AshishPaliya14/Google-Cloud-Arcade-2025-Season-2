# Google-Cloud-Arcade-2025

# Create a Python Artifact Registry and Upload Code || Lab Solution || gem-artifact-registry-python || Google Cloud Arcade 2025 🎮

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

gcloud artifacts repositories create my-python-repo \
    --repository-format=python \
    --location="$REGION" \
    --description="Python package repository"

pip install keyrings.google-artifactregistry-auth

pip config set global.extra-index-url https://"$REGION"-python.pkg.dev/"$PROJECT_ID"/my-python-repo/simple

mkdir my-package
cd my-package

cat > setup.py <<EOF
from setuptools import setup, find_packages

setup(
    name='my_package',
    version='0.1.0',
    author='cls',
    author_email='"$EMAIL"',
    packages=find_packages(exclude=['tests']),
    install_requires=[
        # List your dependencies here
    ],
    description='A sample Python package',
)
EOF

mkdir -p my_package
cat > my_package/my_module.py <<EOF
def hello_world():
    return 'Hello, world!'
EOF

pip install twine

python setup.py sdist bdist_wheel

python3 -m twine upload --repository-url https://"$REGION"-python.pkg.dev/"$PROJECT_ID"/my-python-repo/ dist/*

gcloud artifacts packages list --repository=my-python-repo --location="$REGION"
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
