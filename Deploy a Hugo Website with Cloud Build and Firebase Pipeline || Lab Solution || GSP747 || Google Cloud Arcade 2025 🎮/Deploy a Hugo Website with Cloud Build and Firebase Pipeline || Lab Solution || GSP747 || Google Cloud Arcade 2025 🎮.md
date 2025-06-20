# Google-Cloud-Arcade-2025

# Deploy a Hugo Website with Cloud Build and Firebase Pipeline || Lab Solution || GSP747 || Google Cloud Arcade 2025 🎮

## 💡 Solution here


### 📍 Go to Compute Engine > VM Instances > click SSH key of hugo-dev-vm

```
#!/bin/bash

# Bright Foreground Colors
BLACK_TEXT=$'\033[0;90m'
RED_TEXT=$'\033[0;91m'
GREEN_TEXT=$'\033[0;92m'
YELLOW_TEXT=$'\033[0;93m'
BLUE_TEXT=$'\033[0;94m'
MAGENTA_TEXT=$'\033[0;95m'
CYAN_TEXT=$'\033[0;96m'
WHITE_TEXT=$'\033[0;97m'

NO_COLOR=$'\033[0m'
RESET_FORMAT=$'\033[0m'
BOLD_TEXT=$'\033[1m'
UNDERLINE_TEXT=$'\033[4m'

# Displaying start message
echo
echo "${CYAN_TEXT}${BOLD_TEXT}╔════════════════════════════════════════════════════════╗${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}|                      LearnWithAshish                   |${RESET_FORMAT}"
echo "${CYAN_TEXT}${BOLD_TEXT}╚════════════════════════════════════════════════════════╝${RESET_FORMAT}"
echo

cat /tmp/installhugo.sh

cd ~
/tmp/installhugo.sh

export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format="value(projectNumber)")
export REGION=$(gcloud compute project-info describe \
--format="value(commonInstanceMetadata.items[google-compute-default-region])")
sudo apt-get update -y
sudo apt-get install git -y
sudo apt-get install gh -y

curl -sS https://webi.sh/gh | sh

gh auth login
gh api user -q ".login"
GITHUB_USERNAME=$(gh api user -q ".login")
git config --global user.name "${GITHUB_USERNAME}"
git config --global user.email "${USER_EMAIL}"
echo ${GITHUB_USERNAME}
echo ${USER_EMAIL}

cd ~
gh repo create  my_hugo_site --private 
gh repo clone  my_hugo_site 

cd ~
/tmp/hugo new site my_hugo_site --force

cd ~/my_hugo_site
git clone \
  https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng
echo 'theme = "hello-friend-ng"' >> config.toml

sudo rm -r themes/hello-friend-ng/.git
sudo rm themes/hello-friend-ng/.gitignore 

cd ~/my_hugo_site
/tmp/hugo server -D --bind 0.0.0.0 --port 8080

```

![LearnWithAshish](https://github.com/user-attachments/assets/72e4b3b5-ad56-4abb-8a97-0a4a7a5acecb)

- 📍 **After getting score on Task 1, press CTRL+C to stop the Hugo server**
---

```
export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format="value(projectNumber)")
export REGION=$(gcloud compute project-info describe \
--format="value(commonInstanceMetadata.items[google-compute-default-region])")

curl -sL https://firebase.tools | bash

cd ~/my_hugo_site
firebase init

/tmp/hugo && firebase deploy

git config --global user.name "hugo"
git config --global user.email "hugo@blogger.com"

cd ~/my_hugo_site
echo "resources" >> .gitignore

git add .
git commit -m "Add app to GitHub Repository"
git push -u origin master

cd ~/my_hugo_site
cp /tmp/cloudbuild.yaml .

cat cloudbuild.yaml

echo $REGION
echo $PROJECT_ID

gcloud builds connections create github cloud-build-connection --project=$PROJECT_ID  --region=$REGION 

gcloud builds connections describe cloud-build-connection --region=$REGION 

```

---
- 📍 **Use Up and Down key and press Spacebar key to select the below image checkpoint option**
 
![LearnWithAshish](https://github.com/user-attachments/assets/bf227f9a-37ec-436f-bedb-a5b09758d381)


---

- 📍 **Now select the use an existing project and press enter key**

![LearnWithAshish](https://github.com/user-attachments/assets/6e5b8b42-b21d-4d25-b93d-7db7baff373a)

---

- 📍 **Now select the currect project and press enter key**

![LearnWithAshish](https://github.com/user-attachments/assets/72a89589-c623-4696-9c9d-ede0d7c412bb)


- 📍 **Now press the enter key few times**
---

```
export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format="value(projectNumber)")
export REGION=$(gcloud compute project-info describe \
--format="value(commonInstanceMetadata.items[google-compute-default-region])")

read -p "Enter the GITHUB_USERNAME : " GITHUB_USERNAME

echo $GITHUB_USERNAME

gcloud builds repositories create hugo-website-build-repository \
  --remote-uri="https://github.com/$GITHUB_USERNAME/my_hugo_site.git" \
  --connection="cloud-build-connection" --region=$REGION

gcloud builds triggers create github --name="commit-to-master-branch1" \
   --repository=projects/$PROJECT_ID/locations/$REGION/connections/cloud-build-connection/repositories/hugo-website-build-repository \
   --build-config='cloudbuild.yaml' \
   --service-account=projects/$PROJECT_ID/serviceAccounts/$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
   --region=$REGION \
   --branch-pattern='^master$'

cd ~/my_hugo_site

sed -i "3c\title = 'Blogging with Hugo and Cloud Build'" config.toml

git add .
git commit -m "I updated the site title"
git push -u origin master

cd ~/my_hugo_site
cp /tmp/cloudbuild.yaml .

cat cloudbuild.yaml

echo $REGION

gcloud builds submit --region=$REGION

sleep 30

echo $REGION

BUILD_ID=$(gcloud builds list --region=$REGION --format="value(ID)" --limit=1)
gcloud builds log $BUILD_ID --region=$REGION

gcloud builds log "$(gcloud builds list --region=$REGION --format='value(ID)' --limit=1)" | grep "Hosting URL"
BUILD_ID=$(gcloud builds list --region=$REGION --format="value(ID)" --limit=1)
gcloud builds log $BUILD_ID --region=$REGION

gcloud builds log "$(gcloud builds list --region=$REGION --format='value(ID)' --limit=1)" | grep "Hosting URL"

echo ""

echo $REGION

echo ""
echo
echo -e "\e[41;97m🎉${WHITE}${BOLD} Congratulations for completing the Lab! 🎉 \e[0m"
echo
echo "📺 Subscribe to the channel: https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ"
echo

```
---

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
