# Google-Cloud-Arcade-2025

# Block.one: Getting Started with The EOSIO Blockchain || Lab Solution || GSP873 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell
---

<div style="padding: 15px; margin: 10px 0;">
<p><strong>>> Command for creating a VM named "my-vm-1" : </strong></p>

```
#!/bin/bash


echo "===================================================="
echo "                 LearnWithAshish                    "
echo "===================================================="
echo

# Prompt user for region and zone
read -p ">> Enter the region (e.g. us-central1): " REGION
read -p ">> Enter the zone (e.g. us-central1-a): " ZONE

VM_NAME="my-vm-1"
REGION="europe-west1"
ZONE="europe-west1-b"
MACHINE_TYPE="e2-standard-2"
IMAGE_PROJECT="ubuntu-os-cloud"
IMAGE_NAME="ubuntu-2404-minimal-v20240606"  # As of June 2024; adjust if newer needed

# Optional: Set gcloud config defaults
gcloud config set compute/region $REGION
gcloud config set compute/zone $ZONE

# Create the VM instance with Ubuntu 24.04 LTS Minimal
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --image=$IMAGE_NAME \
  --image-project=$IMAGE_PROJECT \
  --boot-disk-size=10GB \
  --boot-disk-type=pd-balanced \
  --boot-disk-device-name=$VM_NAME

echo "✅ VM $VM_NAME creation initiated in region $REGION, zone $ZONE using image $IMAGE_NAME."
```
</div>

<div style="padding: 15px; margin: 10px 0;">
<p><strong>>> Run this command after creating VM instance : </strong></p>

```
gcloud compute ssh my-vm-1 --zone ZONE

```
```
#!/bin/bash

echo "🔄 Updating package lists..."
sudo apt update -y

echo
echo "⬇️ Downloading EOSIO binary..."
curl -LO https://github.com/eosio/eos/releases/download/v2.1.0/eosio_2.1.0-1-ubuntu-20.04_amd64.deb

echo
echo "💾 Installing EOSIO..."
sudo apt install -y ./eosio_2.1.0-1-ubuntu-20.04_amd64.deb

echo
echo "✅ Verifying installation..."
nodeos --version
cleos version client
keosd -v

echo
echo "🚀 Starting nodeos in background..."
nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin --contracts-console >> nodeos.log 2>&1 &

sleep 5
echo
echo "📡 nodeos is running. Showing logs..."
tail -n 10 nodeos.log

echo
echo "💼 Creating wallet..."
cleos wallet create --name my_wallet --file my_wallet_password

echo
echo "🔑 Viewing wallet password..."
cat my_wallet_password

echo
echo "🔓 Unlocking wallet..."
wallet_password=$(cat my_wallet_password)
cleos wallet open --name my_wallet
cleos wallet unlock --name my_wallet --password $wallet_password

echo
echo "🔐 Importing EOSIO system private key..."
cleos wallet import --name my_wallet --private-key 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3

echo
echo "⬇️ Downloading EOSIO CDT..."
curl -LO https://github.com/eosio/eosio.cdt/releases/download/v1.8.1/eosio.cdt_1.8.1-1-ubuntu-20.04_amd64.deb

echo
echo "💾 Installing EOSIO CDT..."
sudo apt install -y ./eosio.cdt_1.8.1-1-ubuntu-20.04_amd64.deb

echo
echo "✅ Verifying CDT installation..."
eosio-cpp --version

echo
echo "🧪 Unlocking wallet again to ensure access..."
cleos wallet open --name my_wallet
cleos wallet unlock --name my_wallet --password $wallet_password

echo
echo "🔐 Creating new keypair..."
cleos create key --file my_keypair1
cat my_keypair1

# Extract private key from file
user_private_key=$(grep "Private key:" my_keypair1 | cut -d ' ' -f 3)
user_public_key=$(grep "Public key:" my_keypair1 | cut -d ' ' -f 3)

echo
echo "🔐 Importing user private key..."
cleos wallet import --name my_wallet --private-key $user_private_key

echo
echo "👤 Creating EOSIO account named 'bob' with the new public key..."
cleos create account eosio bob $user_public_key

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
