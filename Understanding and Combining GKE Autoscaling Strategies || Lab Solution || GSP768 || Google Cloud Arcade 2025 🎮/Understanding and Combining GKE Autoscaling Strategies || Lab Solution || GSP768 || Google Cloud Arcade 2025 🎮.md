# Google-Cloud-Arcade-2025

# Understanding and Combining GKE Autoscaling Strategies || Lab Solution || GSP768 || Google Cloud Arcade 2025 🎮

## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
export ZONE=
```
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

export PROJECT_ID=$(gcloud config get-value project)

export PROJECT_ID=$DEVSHELL_PROJECT_ID

gcloud config set compute/zone $ZONE

export REGION=${ZONE%-*}
gcloud config set compute/region $REGION

gcloud container clusters create scaling-demo --num-nodes=3 --enable-vertical-pod-autoscaling

cat << EOF > php-apache.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: php-apache
spec:
  selector:
    matchLabels:
      run: php-apache
  replicas: 3
  template:
    metadata:
      labels:
        run: php-apache
    spec:
      containers:
      - name: php-apache
        image: k8s.gcr.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
---
apiVersion: v1
kind: Service
metadata:
  name: php-apache
  labels:
    run: php-apache
spec:
  ports:
  - port: 80
  selector:
    run: php-apache
EOF

kubectl apply -f php-apache.yaml

kubectl get deployment

kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

kubectl get hpa

gcloud container clusters describe scaling-demo | grep ^verticalPodAutoscaling -A 1

kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0

kubectl get deployment hello-server

kubectl set resources deployment hello-server --requests=cpu=450m

kubectl describe pod hello-server | sed -n "/Containers:$/,/Conditions:/p"

cat << EOF > hello-vpa.yaml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: hello-server-vpa
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind:       Deployment
    name:       hello-server
  updatePolicy:
    updateMode: "Off"
EOF

kubectl apply -f hello-vpa.yaml

kubectl describe vpa hello-server-vpa

sed -i 's/Off/Auto/g' hello-vpa.yaml
kubectl apply -f hello-vpa.yaml

kubectl scale deployment hello-server --replicas=2

kubectl get pods

kubectl get hpa

kubectl describe pod hello-server | sed -n "/Containers:$/,/Conditions:/p"

gcloud beta container clusters update scaling-demo --enable-autoscaling --min-nodes 1 --max-nodes 5

gcloud beta container clusters update scaling-demo \
--autoscaling-profile optimize-utilization

gcloud beta container clusters update scaling-demo \
--autoscaling-profile optimize-utilization

kubectl create poddisruptionbudget kube-dns-pdb --namespace=kube-system --selector k8s-app=kube-dns --max-unavailable 1
kubectl create poddisruptionbudget prometheus-pdb --namespace=kube-system --selector k8s-app=prometheus-to-sd --max-unavailable 1
kubectl create poddisruptionbudget kube-proxy-pdb --namespace=kube-system --selector component=kube-proxy --max-unavailable 1
kubectl create poddisruptionbudget metrics-agent-pdb --namespace=kube-system --selector k8s-app=gke-metrics-agent --max-unavailable 1
kubectl create poddisruptionbudget metrics-server-pdb --namespace=kube-system --selector k8s-app=metrics-server --max-unavailable 1
kubectl create poddisruptionbudget fluentd-pdb --namespace=kube-system --selector k8s-app=fluentd-gke --max-unavailable 1
kubectl create poddisruptionbudget backend-pdb --namespace=kube-system --selector k8s-app=glbc --max-unavailable 1
kubectl create poddisruptionbudget kube-dns-autoscaler-pdb --namespace=kube-system --selector k8s-app=kube-dns-autoscaler --max-unavailable 1
kubectl create poddisruptionbudget stackdriver-pdb --namespace=kube-system --selector app=stackdriver-metadata-agent --max-unavailable 1
kubectl create poddisruptionbudget event-pdb --namespace=kube-system --selector k8s-app=event-exporter --max-unavailable 1

kubectl get nodes

gcloud container clusters update scaling-demo \
    --enable-autoprovisioning \
    --min-cpu 1 \
    --min-memory 2 \
    --max-cpu 45 \
    --max-memory 160

cat << EOF > pause-pod.yaml
---
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: overprovisioning
value: -1
globalDefault: false
description: "Priority class used by overprovisioning."
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: overprovisioning
  namespace: kube-system
spec:
  replicas: 1
  selector:
    matchLabels:
      run: overprovisioning
  template:
    metadata:
      labels:
        run: overprovisioning
    spec:
      priorityClassName: overprovisioning
      containers:
      - name: reserve-resources
        image: k8s.gcr.io/pause
        resources:
          requests:
            cpu: 1
            memory: 4Gi
EOF

kubectl apply -f pause-pod.yaml

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
