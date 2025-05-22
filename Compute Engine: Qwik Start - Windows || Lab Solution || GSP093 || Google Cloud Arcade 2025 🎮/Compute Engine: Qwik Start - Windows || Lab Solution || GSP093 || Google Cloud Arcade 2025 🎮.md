# Google-Cloud-Arcade-2025

# Compute Engine: Qwik Start - Windows || Lab Solution || GSP093 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell


```bash
#!/bin/bash

# Prompt user for region and zone
read -p "Enter your region (e.g., us-central1): " REGION
read -p "Enter your zone (e.g., us-central1-a): " ZONE

# Set variables
VM_NAME="windows-vm"
IMAGE_PROJECT="windows-cloud"
IMAGE_FAMILY="windows-2022"
MACHINE_TYPE="n1-standard-1"

echo "Creating a Windows VM instance in $ZONE..."

# Create the VM
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --image-project=$IMAGE_PROJECT \
  --image-family=$IMAGE_FAMILY \
  --boot-disk-size=50GB

echo "Enabling RDP (port 3389) in default firewall rule..."
gcloud compute firewall-rules create allow-rdp \
  --allow tcp:3389 \
  --source-ranges=0.0.0.0/0 \
  --target-tags=rdp-access \
  --description="Allow RDP access"

# Add network tag to VM
echo "Adding 'rdp-access' network tag to the VM..."
gcloud compute instances add-tags $VM_NAME \
  --tags=rdp-access \
  --zone=$ZONE

# Wait for the instance to be ready for RDP
echo "⏳ Waiting for Windows Server to initialize..."
echo "Please run the following command periodically to check readiness:"
echo "gcloud compute instances get-serial-port-output $VM_NAME --zone=$ZONE"
echo "Look for the line: 'Instance setup finished. instance is ready to use.'"

# Prompt for setting the Windows password
read -p "Enter your desired Windows username (e.g., admin): " WIN_USER

echo "🔐 Setting/resetting Windows password for user '$WIN_USER'..."
gcloud compute reset-windows-password $VM_NAME --zone $ZONE --user $WIN_USER

echo -e "\e[41;97m🎉 Congratulations for completing the Lab! 🎉 \e[0m"

echo "📺 Subscribe to the channel: https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ"

```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
