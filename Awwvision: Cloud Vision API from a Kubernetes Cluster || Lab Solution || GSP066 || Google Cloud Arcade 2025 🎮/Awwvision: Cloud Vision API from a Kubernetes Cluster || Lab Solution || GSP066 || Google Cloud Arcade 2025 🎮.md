# Google-Cloud-Arcade-2025

# Awwvision: Cloud Vision API from a Kubernetes Cluster || Lab Solution || GSP066 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
gcloud config set project 
```
```
#!/bin/bash

# Bright Foreground Colors
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

# Displaying start message
echo
echo "${CYAN}${BOLD}╔════════════════════════════════════════════════════════╗${RESET}"
echo "${CYAN}${BOLD}|                      LearnWithAshish                   |${RESET}"
echo "${CYAN}${BOLD}╚════════════════════════════════════════════════════════╝${RESET}"
echo

# Get Zone Input
echo "${YELLOW}${BOLD}Please enter your preferred zone (e.g., us-central1-a): ${RESET}"
read ZONE
export ZONE

echo
echo "${YELLOW}${BOLD}Starting${RESET}" "${GREEN}${BOLD}Execution${RESET}"
echo

# Set compute zone
echo "${BLUE}${BOLD}Setting compute zone to ${ZONE}...${RESET}"
gcloud config set compute/zone $ZONE

# Create GKE cluster
echo
echo "${BLUE}${BOLD}Creating GKE cluster 'awwvision'...${RESET}"
gcloud container clusters create awwvision \
    --num-nodes 2 \
    --scopes cloud-platform

# Get cluster credentials
echo
echo "${BLUE}${BOLD}Getting cluster credentials...${RESET}"
gcloud container clusters get-credentials awwvision

# Display cluster info
echo
echo "${BLUE}${BOLD}Cluster information:${RESET}"
kubectl cluster-info

# Install virtualenv
echo
echo "${BLUE}${BOLD}Installing virtualenv...${RESET}"
sudo apt-get install -y virtualenv

# Create Python virtual environment
echo
echo "${BLUE}${BOLD}Creating Python virtual environment...${RESET}"
python3 -m venv venv

# Activate virtual environment
echo
echo "${BLUE}${BOLD}Activating virtual environment...${RESET}"
source venv/bin/activate

# Copy cloud vision files
echo
echo "${BLUE}${BOLD}Copying Cloud Vision files...${RESET}"
gsutil -m cp -r gs://spls/gsp066/cloud-vision .

# Change directory and run make
echo
echo "${BLUE}${BOLD}Setting up awwvision application...${RESET}"
cd cloud-vision/python/awwvision
make all

# Check pod status
echo
echo "${BLUE}${BOLD}Checking pod status:${RESET}"
kubectl get pods

echo
echo "${BLUE}${BOLD}Waiting for pods to initialize...${RESET}"
sleep 5 

echo
echo "${BLUE}${BOLD}Checking pod status again:${RESET}"
kubectl get pods

# Display deployment info
echo
echo "${BLUE}${BOLD}Deployment information:${RESET}"
kubectl get deployments -o wide

# Display service info
echo
echo "${BLUE}${BOLD}Service information:${RESET}"
kubectl get svc awwvision-webapp

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
