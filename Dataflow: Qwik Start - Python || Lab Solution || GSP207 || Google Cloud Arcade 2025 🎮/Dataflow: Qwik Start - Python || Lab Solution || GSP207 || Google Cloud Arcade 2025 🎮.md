# Google-Cloud-Arcade-2025

# Dataflow: Qwik Start - Python || Lab Solution || GSP207 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

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
gcloud auth list

gcloud services disable dataflow.googleapis.com

gcloud services enable dataflow.googleapis.com

export ZONE=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-zone])")

export REGION=$(gcloud compute project-info describe --format="value(commonInstanceMetadata.items[google-compute-default-region])")

gcloud config set compute/region $REGION

export PROJECT_ID=$(gcloud config get-value project)

gcloud storage buckets create gs://$DEVSHELL_PROJECT_ID-bucket --project=$PROJECT_ID --location=us

cat > Dockerfile <<EOF_CP

FROM python:3.9

ARG $DEVSHELL_PROJECT_ID
ARG $REGION

RUN pip install 'apache-beam[gcp]'==2.42.0

ENV BUCKET=gs://\${BUCKET_NAME}-bucket

COPY run_beam.sh /run_beam.sh

RUN chmod +x /run_beam.sh

CMD ["/run_beam.sh"]
EOF_CP

cat > run_beam.sh <<EOF_CP
#!/bin/bash

export DEVSHELL_PROJECT_ID=\${DEVSHELL_PROJECT_ID}
export REGION=\${REGION}
export BUCKET=gs://\${DEVSHELL_PROJECT_ID}-bucket

python -m apache_beam.examples.wordcount --output OUTPUT_FILE

python -m apache_beam.examples.wordcount --project \$DEVSHELL_PROJECT_ID \
  --runner DataflowRunner \
  --staging_location \$BUCKET/staging \
  --temp_location \$BUCKET/temp \
  --output \$BUCKET/results/output \
  --region \$REGION
EOF_CP

docker build --build-arg DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID --build-arg REGION=$REGION -t beam-dataflow:latest .

#!/bin/bash
while true; do
    docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID -e REGION=$REGION beam-dataflow:latest

    if [ $? -eq 0 ]; then
        echo "Dataflow job completed and succeeded..."
        break
    else
        echo "job retrying. Subscribe to techcps https://www.youtube.com/@techcps"
        sleep 10
    fi
done

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
