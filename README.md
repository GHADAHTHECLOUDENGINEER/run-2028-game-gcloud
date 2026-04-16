# 🚀 Cloud Run Deployment Lab

### 1. Create Repository
gcloud artifacts repositories create my-lab-repo \
    --repository-format=docker \
    --location=us-central1

### 2. Authenticate Docker
gcloud auth configure-docker us-central1-docker.pkg.dev

### 3. Prepare Image (Pull & Tag)
docker pull kubespheredev/2048
docker tag kubespheredev/2048 us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1

### 4. Push to Registry
docker push us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1

### 5. Deploy to Cloud Run
gcloud run deploy my-lab-service \
    --image us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1 \
    --region us-central1 \
    --allow-unauthenticated \
    --port 80

---
✅ Done! Open the Service URL to play the game.
