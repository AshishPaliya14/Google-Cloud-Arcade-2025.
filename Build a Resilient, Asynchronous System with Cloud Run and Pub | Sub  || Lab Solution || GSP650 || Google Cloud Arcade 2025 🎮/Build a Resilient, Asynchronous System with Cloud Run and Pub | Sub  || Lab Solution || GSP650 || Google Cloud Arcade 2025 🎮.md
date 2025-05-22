# Google-Cloud-Arcade-2025

# Build a Resilient, Asynchronous System with Cloud Run and Pub | Sub  || Lab Solution || GSP650 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell


```
set -e

echo "Build a Resilient, Asynchronous System with Cloud Run and Pub / Sub"

# Prompt user for project, region, zone
read -p "Enter your Google Cloud Project ID: " PROJECT_ID
read -p "Enter the region to deploy (e.g. us-central1): " REGION
read -p "Enter the zone (if needed, else press Enter to skip): " ZONE

export GOOGLE_CLOUD_PROJECT=$PROJECT_ID

echo ""
echo "Using Project ID: $GOOGLE_CLOUD_PROJECT"
echo "Using Region: $REGION"
if [ -n "$ZONE" ]; then
  echo "Using Zone: $ZONE"
fi
echo ""

# Set gcloud config project and compute region/zone
gcloud config set project $GOOGLE_CLOUD_PROJECT
gcloud config set run/region $REGION
if [ -n "$ZONE" ]; then
  gcloud config set compute/zone $ZONE
fi

# Enable required services
gcloud services enable run.googleapis.com
gcloud services enable pubsub.googleapis.com
gcloud services enable cloudbuild.googleapis.com

# ============================
# Task 1: Create Pub/Sub topic
# ============================
echo "Creating Pub/Sub topic..."
if ! gcloud pubsub topics describe new-lab-report &>/dev/null; then
  gcloud pubsub topics create new-lab-report
else
  echo "Topic new-lab-report already exists."
fi

# ============================
# Task 2: Deploy Lab Report Service
# ============================
echo "Deploying Lab Report Service..."
LAB_DIR=~/pet-theory/lab-report-service
mkdir -p $LAB_DIR
cd $LAB_DIR

cat > package.json <<EOF
{
  "name": "lab-report-service",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "body-parser": "^1.20.0",
    "@google-cloud/pubsub": "^3.0.0"
  }
}
EOF

cat > index.js <<'EOF'
const express = require('express');
const bodyParser = require('body-parser');
const {PubSub} = require('@google-cloud/pubsub');
const pubsub = new PubSub();

const app = express();
app.use(bodyParser.json());

app.post('/', async (req, res) => {
  const labReport = req.body;
  const dataBuffer = Buffer.from(JSON.stringify(labReport));
  await pubsub.topic('new-lab-report').publish(dataBuffer);
  console.log('Lab report published to Pub/Sub.');
  res.status(204).send();
});

const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Listening on port ${PORT}`);
});
EOF

cat > Dockerfile <<EOF
FROM node:18
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
CMD ["npm", "start"]
EOF

cat > deploy.sh <<EOF
gcloud builds submit --tag gcr.io/\$GOOGLE_CLOUD_PROJECT/lab-report-service
gcloud run deploy lab-report-service \
  --image gcr.io/\$GOOGLE_CLOUD_PROJECT/lab-report-service \
  --platform managed \
  --region $REGION \
  --allow-unauthenticated \
  --max-instances=1
EOF

chmod +x deploy.sh
./deploy.sh

# ============================
# Task 3: Deploy Email Service
# ============================
echo "Deploying Email Service..."
EMAIL_DIR=~/pet-theory/email-service
mkdir -p $EMAIL_DIR
cd $EMAIL_DIR

cat > package.json <<EOF
{
  "name": "email-service",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "body-parser": "^1.20.0"
  }
}
EOF

cat > index.js <<'EOF'
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

app.post('/', (req, res) => {
  const report = JSON.parse(Buffer.from(req.body.message.data, 'base64').toString());
  console.log(`Sending email for report ID: ${report.id}`);
  res.status(204).send();
});

const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`Email service running on port ${PORT}`);
});
EOF

cat > Dockerfile <<EOF
FROM node:18
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
CMD ["npm", "start"]
EOF

cat > deploy.sh <<EOF
gcloud builds submit --tag gcr.io/\$GOOGLE_CLOUD_PROJECT/email-service
gcloud run deploy email-service \
  --image gcr.io/\$GOOGLE_CLOUD_PROJECT/email-service \
  --platform managed \
  --region $REGION \
  --no-allow-unauthenticated \
  --max-instances=1
EOF

chmod +x deploy.sh
./deploy.sh

EMAIL_URL=$(gcloud run services describe email-service --region $REGION --format='value(status.url)')

# Create service account for Pub/Sub push
gcloud iam service-accounts create pubsub-invoker --display-name="PubSub Push Invoker"
gcloud run services add-iam-policy-binding email-service \
  --member=serviceAccount:pubsub-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com \
  --role=roles/run.invoker --region $REGION

gcloud pubsub subscriptions create email-sub \
  --topic=new-lab-report \
  --push-endpoint=$EMAIL_URL \
  --push-auth-service-account=pubsub-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com

# ============================
# Task 4: Deploy SMS Service
# ============================
echo "Deploying SMS Service..."
SMS_DIR=~/pet-theory/sms-service
mkdir -p $SMS_DIR
cd $SMS_DIR

cat > package.json <<EOF
{
  "name": "sms-service",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "body-parser": "^1.20.0"
  }
}
EOF

cat > index.js <<'EOF'
const express = require('express');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

app.post('/', (req, res) => {
  const report = JSON.parse(Buffer.from(req.body.message.data, 'base64').toString());
  console.log(`Sending SMS for report ID: ${report.id}`);
  res.status(204).send();
});

const PORT = process.env.PORT || 8080;
app.listen(PORT, () => {
  console.log(`SMS service running on port ${PORT}`);
});
EOF

cat > Dockerfile <<EOF
FROM node:18
WORKDIR /usr/src/app
COPY package.json ./
RUN npm install
COPY . .
CMD ["npm", "start"]
EOF

cat > deploy.sh <<EOF
gcloud builds submit --tag gcr.io/\$GOOGLE_CLOUD_PROJECT/sms-service
gcloud run deploy sms-service \
  --image gcr.io/\$GOOGLE_CLOUD_PROJECT/sms-service \
  --platform managed \
  --region $REGION \
  --no-allow-unauthenticated \
  --max-instances=1
EOF

chmod +x deploy.sh
./deploy.sh

SMS_URL=$(gcloud run services describe sms-service --region $REGION --format='value(status.url)')

gcloud run services add-iam-policy-binding sms-service \
  --member=serviceAccount:pubsub-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com \
  --role=roles/run.invoker --region $REGION

gcloud pubsub subscriptions create sms-sub \
  --topic=new-lab-report \
  --push-endpoint=$SMS_URL \
  --push-auth-service-account=pubsub-invoker@$GOOGLE_CLOUD_PROJECT.iam.gserviceaccount.com

echo -e "\e[41;97m🎉 Congratulations for completing the Lab! 🎉 \e[0m"

echo "📺 Subscribe to the channel: https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ"


```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
