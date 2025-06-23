# Google-Cloud-Arcade-2025

# Multiple VPC Networks || Lab Solution || GSP211 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
#!/bin/bash

# Bright Foreground Colors
BLACK_TEXT=$'\033[0;90m'
RED_TEXT=$'\033[0;91m'
GREEN_TEXT=$'\033[0;92m'
YELLOW=$'\033[0;93m'
BLUE_TEXT=$'\033[0;94m'
MAGENTA_TEXT=$'\033[0;95m'
CYAN_TEXT=$'\033[0;96m'
WHITE_TEXT=$'\033[0;97m'

NO_COLOR=$'\033[0m'
RESET=$'\033[0m'
BOLD=$'\033[1m'
UNDERLINE_TEXT=$'\033[4m'

# Displaying start message
echo
echo "${CYAN_TEXT}${BOLD}╔════════════════════════════════════════════════════════╗${RESET}"
echo "${CYAN_TEXT}${BOLD}|                      LearnWithAshish                   |${RESET}"
echo "${CYAN_TEXT}${BOLD}╚════════════════════════════════════════════════════════╝${RESET}"
echo
echo "Please set the below values correctly"
read -p "${YELLOW}${BOLD}Enter the ZONE: ${RESET}" ZONE
read -p "${YELLOW}${BOLD}Enter the REGION_2: ${RESET}" REGION_2
export ZONE REGION_2

export REGION="${ZONE%-*}"

export PROJECT_ID=$(gcloud config get-value project)

gcloud compute networks create managementnet --subnet-mode=custom

gcloud compute networks subnets create managementsubnet-1 --network=managementnet --region=$REGION --range=10.130.0.0/20

gcloud compute networks create privatenet --subnet-mode=custom

gcloud compute networks subnets create privatesubnet-1 --network=privatenet --region=$REGION --range=172.16.0.0/24

gcloud compute networks subnets create privatesubnet-2 --network=privatenet --region=$REGION_2 --range=172.20.0.0/20

gcloud compute networks list

gcloud compute networks subnets list --sort-by=NETWORK

gcloud compute firewall-rules create managementnet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=managementnet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0

gcloud compute firewall-rules create privatenet-allow-icmp-ssh-rdp --direction=INGRESS --priority=1000 --network=privatenet --action=ALLOW --rules=icmp,tcp:22,tcp:3389 --source-ranges=0.0.0.0/0

gcloud compute firewall-rules list --sort-by=NETWORK

gcloud compute instances create managementnet-vm-1 --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --machine-type=e2-micro --subnet=managementsubnet-1

gcloud compute instances create privatenet-vm-1 --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --machine-type=e2-micro --subnet=privatesubnet-1

gcloud compute instances create vm-appliance --project=$DEVSHELL_PROJECT_ID --zone=$ZONE --machine-type=e2-standard-4 --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=privatesubnet-1 --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=managementsubnet-1 --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=mynetwork

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
