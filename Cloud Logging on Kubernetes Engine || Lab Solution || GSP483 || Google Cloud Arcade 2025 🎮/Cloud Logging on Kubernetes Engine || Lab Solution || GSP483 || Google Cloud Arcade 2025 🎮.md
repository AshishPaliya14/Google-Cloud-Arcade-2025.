# Google-Cloud-Arcade-2025

# Cloud Logging on Kubernetes Engine || Lab Solution || GSP483 || Google Cloud Arcade 2025 🎮

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
NC='\033[0m' # No Color

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

echo "${BG_CYAN}${BOLD}==========================================${RESET}"
echo "${BG_CYAN}${BOLD}               LearnWithAshish            ${RESET}"
echo "${BG_CYAN}${BOLD}==========================================${RESET}"

# Function to set and export zone
set_zone() {
    echo -e "${YELLOW}🌍 Configuring Zone Settings${NC}"
    
    # Try to get default zone
    export ZONE=$(gcloud config get-value compute/zone 2>/dev/null)
    
    if [ -z "$ZONE" ]; then
        echo -e "${YELLOW}No default zone configured.${NC}"
        echo -e "${CYAN}Available zones in your project:${NC}"
        gcloud compute zones list --format="value(name)" | sort | pr -3 -t
        
        while true; do
            read -p "${WHITE}Enter your preferred zone (e.g. us-central1-a):" ZONE
            if gcloud compute zones describe $ZONE &>/dev/null; then
                break
            else
                echo -e "${RED}Invalid zone. Please try again.${NC}"
            fi
        done
        
        # Set zone in gcloud config
        gcloud config set compute/zone $ZONE
    fi
    
    echo -e "${GREEN}✅ Using zone: ${WHITE}$ZONE${NC}"
    export ZONE
    export REGION="${ZONE%-*}"
    echo -e "${GREEN}✅ Derived region: ${WHITE}$REGION${NC}"
}

# Main execution
set_zone

# Set project configuration
echo -e "\n${YELLOW}⚙️ Configuring Project Settings${NC}"
export PROJECT_ID=$(gcloud config get-value project)
gcloud config set project $DEVSHELL_PROJECT_ID
echo -e "${GREEN}✅ Project set to: ${WHITE}$DEVSHELL_PROJECT_ID${NC}"

# Clone repository
echo -e "\n${YELLOW}📥 Cloning GKE Logging Sinks Demo Repository${NC}"
git clone https://github.com/GoogleCloudPlatform/gke-logging-sinks-demo
sleep 10

cd gke-logging-sinks-demo || {
    echo -e "${RED}❌ Failed to change directory${NC}"
    exit 1
}
sleep 10

# Configure region and zone
echo -e "\n${YELLOW}🌐 Setting Compute Region/Zone${NC}"
gcloud config set compute/region $REGION
gcloud config set compute/zone $ZONE
echo -e "${GREEN}✅ Region: ${WHITE}$REGION${NC}"
echo -e "${GREEN}✅ Zone: ${WHITE}$ZONE${NC}"

# Update Terraform configuration
echo -e "\n${YELLOW}🔄 Updating Terraform Configuration${NC}"
sed -i 's/  version = "~> 2.11.0"/  version = "~> 2.19.0"/g' terraform/provider.tf
sed -i 's/  filter      = "resource.type = container"/  filter      = "resource.type = k8s_container"/g' terraform/main.tf
echo -e "${GREEN}✅ Configuration updated${NC}"

# Execute Terraform
echo -e "\n${YELLOW}🚀 Deploying Infrastructure${NC}"
make create
make validate

# Logging queries
echo -e "\n${YELLOW}🔍 Querying GKE Logs${NC}"
echo -e "${CYAN}Running initial log query...${NC}"
gcloud logging read "resource.type=k8s_container AND resource.labels.cluster_name=stackdriver-logging" --project=$PROJECT_ID

echo -e "\n${CYAN}Running detailed JSON log query...${NC}"
gcloud logging read "resource.type=k8s_container AND resource.labels.cluster_name=stackdriver-logging" --project=$PROJECT_ID --format=json

# Create logging sink
echo -e "\n${YELLOW}📊 Creating BigQuery Logging Sink${NC}"
gcloud logging sinks create gke_logs_sink \
    bigquery.googleapis.com/projects/$PROJECT_ID/datasets/bq_logs \
    --log-filter='resource.type="k8s_container" 
resource.labels.cluster_name="stackdriver-logging"' \
    --include-children \
    --format='json'

sleep 17

# BigQuery query
echo -e "\n${YELLOW}📈 Querying BigQuery Logs${NC}"
bq query --use_legacy_sql=false \
"
SELECT * FROM \`$DEVSHELL_PROJECT_ID.gke_logs_dataset.diagnostic_log_*\`
WHERE _TABLE_SUFFIX BETWEEN FORMAT_DATE('%Y%m%d', CURRENT_DATE() - INTERVAL 1 DAY) AND FORMAT_DATE('%Y%m%d', CURRENT_DATE()) 
"

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
