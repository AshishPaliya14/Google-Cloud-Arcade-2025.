# Google-Cloud-Arcade-2025

# APIs Explorer: Create and Update a Cluster || Lab Solution || GSP288 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
export Region=
```
```
#!/bin/bash

# Color Definitions
BLUE='\033[0;34m'
GREEN='\033[0;32m'
YELLOW='\033[0;33m'
RED='\033[0;31m'
NC='\033[0m' # No Color
BOLD='\033[1m'

# Display Header
clear
echo -e "${BLUE}${BOLD}"
echo "╔════════════════════════════════════════════╗"
echo "║              LearnWithAshish               ║"
echo "╚════════════════════════════════════════════╝"
echo -e "${NC}"
echo -e "${YELLOW}📺 YouTube: [[https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ]]${NC}"
echo -e "${YELLOW}⭐ Please Subscribe for More Cloud Tutorials! ⭐${NC}"
echo


  gcloud auth list

  export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")

  export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

  gcloud config set compute/zone "$ZONE"

  gcloud config set compute/region "$Region"

  gcloud config set project "$DEVSHELL_PROJECT_ID"

  gcloud services enable dataproc.googleapis.com

  curl -X GET \
  -H "Authorization: Bearer $(gcloud auth print-access-token)" \
  https://www.googleapis.com/compute/v1/projects/$DEVSHELL_PROJECT_ID/zones/$ZONE


  gcloud dataproc clusters create my-cluster --project=$DEVSHELL_PROJECT_ID --region $REGION --zone $ZONE --image-version=2.0-debian10 --optional-components=JUPYTER

  gcloud dataproc jobs submit spark \
      --project=$DEVSHELL_PROJECT_ID \
      --region=$REGION \
      --cluster=my-cluster \
      --class=org.apache.spark.examples.SparkPi \
      --jars=file:///usr/lib/spark/examples/jars/spark-examples.jar \
      -- 1000

  gcloud dataproc clusters update my-cluster --project=$DEVSHELL_PROJECT_ID --region $REGION --num-workers=3

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
