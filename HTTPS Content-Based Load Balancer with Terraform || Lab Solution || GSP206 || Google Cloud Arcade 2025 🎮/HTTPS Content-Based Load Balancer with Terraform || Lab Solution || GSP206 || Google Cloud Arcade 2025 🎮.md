# Google-Cloud-Arcade-2025

# Modular Load Balancing with Terraform - Regional Load Balancer || Lab Solution || GSP191 || Google Cloud Arcade 2025 🎮

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

#!/bin/bash


# Modern Color Definitions
BLACK=$(tput setaf 0)
RED=$(tput setaf 1)
GREEN=$(tput setaf 2)
YELLOW=$(tput setaf 3)
BLUE=$(tput setaf 4)
MAGENTA=$(tput setaf 5)
CYAN=$(tput setaf 6)
WHITE=$(tput setaf 7)

BG_BLACK=$(tput setab 0)
BG_RED=$(tput setab 1)
BG_GREEN=$(tput setab 2)
BG_YELLOW=$(tput setab 3)
BG_BLUE=$(tput setab 4)
BG_MAGENTA=$(tput setab 5)
BG_CYAN=$(tput setab 6)
BG_WHITE=$(tput setab 7)

BOLD=$(tput bold)
RESET=$(tput sgr0)

# Box Drawing Characters
BOX_TOP="${CYAN}╔════════════════════════════════════════════╗${RESET}"
BOX_MID="${CYAN}║                                            ║${RESET}"
BOX_BOT="${CYAN}╚════════════════════════════════════════════╝${RESET}"

# Header with Dr. Abhishek branding
clear
echo -e "${BOX_TOP}"
echo -e "${CYAN}║               LearnWithAshish              ║${RESET}"
echo -e "${BOX_BOT}"
echo
echo -e "${WHITE}📺 YouTube: ${BLUE}[https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ]${RESET}"
echo -e "${WHITE}⭐ Subscribe for more Cloud & DevOps tutorials! ⭐${RESET}"
echo

# Get user input for regions
echo -e "${GREEN}${BOLD}🌍 Region Configuration${RESET}"
echo -e "${YELLOW}Please enter the regions for your backend groups:${RESET}"
read -p "Enter Group 1 Region (e.g. us-central1): " group1_region
read -p "Enter Group 2 Region (e.g. us-east1): " group2_region
read -p "Enter Group 3 Region (e.g. europe-west1): " group3_region
echo

# Clone Terraform module
echo -e "${GREEN}${BOLD}📥 Cloning Terraform HTTP Load Balancer module...${RESET}"
git clone https://github.com/terraform-google-modules/terraform-google-lb-http.git
cd ~/terraform-google-lb-http/examples/multi-backend-multi-mig-bucket-https-lb || {
    echo -e "${RED}❌ Failed to change directory${RESET}"
    exit 1
}

# Download configuration
echo -e "${GREEN}${BOLD}⚙️ Downloading Load Balancer configuration...${RESET}"
rm -rf main.tf
wget -q https://raw.githubusercontent.com/quiccklabs/Labs_solutions/master/HTTPS%20Content-Based%20Load%20Balancer%20with%20Terraform/main.tf

# Create variables file
echo -e "${GREEN}${BOLD}📝 Generating Terraform variables file...${RESET}"
cat > variables.tf <<EOF
variable "group1_region" {
  default = "$group1_region"
}

variable "group2_region" {
  default = "$group2_region"
}

variable "group3_region" {
  default = "$group3_region"
}

variable "network_name" {
  default = "ml-bk-ml-mig-bkt-s-lb"
}

variable "project" {
  type = string
}
EOF

# Terraform execution
echo -e "${GREEN}${BOLD}🛠️ Initializing Terraform...${RESET}"
terraform init

echo -e "${GREEN}${BOLD}🔎 Planning infrastructure...${RESET}"
echo $DEVSHELL_PROJECT_ID | terraform plan

echo -e "${GREEN}${BOLD}🚀 Applying configuration (may take 10-15 minutes)...${RESET}"
echo $DEVSHELL_PROJECT_ID | terraform apply -auto-approve

# Get Load Balancer IP
EXTERNAL_IP=$(terraform output | grep load-balancer-ip | cut -d = -f2 | xargs echo -n)

echo
echo -e "${WHITE}Your Load Balancer is now available at:${RESET}"
echo -e "${BLUE}${BOLD}http://${EXTERNAL_IP}${RESET}"
echo

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
