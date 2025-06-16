# Google-Cloud-Arcade-2025

# APIs Explorer: Cloud SQL || Lab Solution || GSP423 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

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

gcloud config set compute/region "$REGION"

gcloud config set project "$DEVSHELL_PROJECT_ID"

gcloud services enable sqladmin.googleapis.com

gcloud sql instances create my-instance --project=$DEVSHELL_PROJECT_ID --region=$REGION --database-version=MYSQL_5_7 --tier=db-n1-standard-1

gcloud sql databases create mysql-db --instance=my-instance --project=$DEVSHELL_PROJECT_ID

bq mk --dataset $DEVSHELL_PROJECT_ID:mysql_db


bq query --use_legacy_sql=false \
"CREATE TABLE \`${DEVSHELL_PROJECT_ID}.mysql_db.info\` (
  name STRING,
  age INT64,
  occupation STRING
);"


cat > employee_info.csv <<EOF_CP
"Sean", 23, "Content Creator"
"Emily", 34, "Cloud Engineer"
"Rocky", 40, "Event coordinator"
"Kate", 28, "Data Analyst"
"Juan", 51, "Program Manager"
"Jennifer", 32, "Web Developer"
EOF_CP


gsutil mb gs://$DEVSHELL_PROJECT_ID

gsutil cp employee_info.csv gs://$DEVSHELL_PROJECT_ID/

SERVICE_EMAIL=$(gcloud sql instances describe my-instance --format="value(serviceAccountEmailAddress)")

gsutil iam ch serviceAccount:$SERVICE_EMAIL:roles/storage.admin gs://$DEVSHELL_PROJECT_ID/

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
