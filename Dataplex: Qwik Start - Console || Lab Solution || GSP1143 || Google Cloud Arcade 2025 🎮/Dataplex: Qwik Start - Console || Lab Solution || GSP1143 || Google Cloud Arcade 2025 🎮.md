# Google-Cloud-Arcade-2025

# Dataplex: Qwik Start - Console || Lab Solution || GSP1143 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell


```bash
read -p "Enter your GCP region (e.g., us-central1): " REGION; PROJECT_ID=$(gcloud config get-value project); LAKE_NAME="sensors"; ZONE_NAME="temperature-raw-data"; BUCKET_NAME="$PROJECT_ID"
gcloud dataplex lakes create $LAKE_NAME --project=$PROJECT_ID --location=$REGION --display-name="sensors" && gcloud dataplex zones create $ZONE_NAME --lake=$LAKE_NAME --project=$PROJECT_ID --location=$REGION --display-name="temperature raw data" --type="RAW" --resource-location-type="SINGLE_REGION" && gsutil mb -l $REGION gs://$BUCKET_NAME/
gcloud dataplex assets create measurements --project=$PROJECT_ID --location=$REGION --lake=$LAKE_NAME --zone=$ZONE_NAME --display-name="measurements" --resource-type="STORAGE_BUCKET" --resource-name="projects/$PROJECT_ID/buckets/$BUCKET_NAME" --discovery-enabled
gcloud dataplex assets delete measurements --project=$PROJECT_ID --location=$REGION --lake=$LAKE_NAME --zone=$ZONE_NAME --quiet && gcloud dataplex zones delete $ZONE_NAME --lake=$LAKE_NAME --project=$PROJECT_ID --location=$REGION --quiet
gcloud dataplex lakes delete $LAKE_NAME --project=$PROJECT_ID --location=$REGION --quiet
```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
