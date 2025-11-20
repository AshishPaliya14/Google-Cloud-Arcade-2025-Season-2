# Google-Cloud-Arcade-2025

# Manage Storage Configuration using gsutil || Lab Solution || Google Cloud Arcade 2025 🎮

## Subscribe : "Learn With Ashish" [![Subscribe on YouTube](https://img.shields.io/badge/-Subscribe%20on%20YouTube-FF0000?style=for-thebadge&logo=youtube&logoColor=white&labelColor=FF0000)](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)   


## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
# Step 1: Get the sample code and set variables
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
cd training-data-analyst/blogs
PROJECT_ID=$(gcloud config get-value project)
BUCKET=${PROJECT_ID}-bucket

# Step 2: Create a bucket
gsutil mb -c multi_regional gs://${BUCKET}

# Step 3: Upload objects to your bucket
gsutil -m cp -r endpointslambda gs://${BUCKET}

# Step 4: List objects in your bucket
gsutil ls gs://${BUCKET}/*

# Step 5: Sync changes with bucket
mv endpointslambda/Apache2_0License.txt endpointslambda/old.txt
rm endpointslambda/aeflex-endpoints/app.yaml
gsutil -m rsync -d -r endpointslambda gs://${BUCKET}/endpointslambda
gsutil ls gs://${BUCKET}/*

# Step 6: Make objects public
gsutil -m acl set -R -a public-read gs://${BUCKET}

# (To test public access, open this link in incognito mode)
# http://storage.googleapis.com/<your-bucket-name>/endpointslambda/old.txt

# Step 7: Copy with different storage class (Nearline)
gsutil cp -s nearline ghcn/ghcn_on_bq.ipynb gs://${BUCKET}

```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
