🚀 Deploying 2048 Game to Google Cloud Run
This project demonstrates the workflow of migrating a container image from Docker Hub to Google Artifact Registry and deploying it as a serverless service using Cloud Run.
🛠 Prerequisites
An active Google Cloud Project.
Google Cloud SDK (gcloud CLI) and Docker installed.
Appropriate permissions (Editor or Owner role).
📋 Step-by-Step Guide
1️⃣ Create the Repository (Artifact Registry)
First, we create a secure "vault" to store our Docker images.
bash
gcloud artifacts repositories create my-lab-repo \
    --repository-format=docker \
    --location=us-central1 \
    --description="My Lab repository for 2048 game"
Use code with caution.
Why? Artifact Registry is the modern evolution of Google Container Registry. It organizes images into logical repositories.
2️⃣ Configure Docker Authentication
We need to tell Docker how to communicate with Google Cloud’s servers.
bash
gcloud auth configure-docker us-central1-docker.pkg.dev
Use code with caution.
Why? This command sets up the necessary credentials so Docker can "Push" images to your private Google Cloud storage.
3️⃣ Prepare the Image (Pull & Tag)
Since we are using an existing game image, we download it first and then rename it to match our project destination.
bash
# Pull the image from Docker Hub
docker pull kubespheredev/2048

# Tag the image for Google Artifact Registry
docker tag kubespheredev/2048 us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1
Use code with caution.
Why the Tag? A Docker Tag acts like a shipping label. It tells Docker exactly which Project, Repository, and Region the image belongs to.
4️⃣ Upload to the Cloud (Push)
Transfer the image files from your local machine to the cloud.
bash
docker push us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1
Use code with caution.
Why? This makes the image accessible to Google Cloud services like Cloud Run.
5️⃣ Final Deployment (Cloud Run)
Spin up the container into a live, scalable web service.
bash
gcloud run deploy my-lab-service \
    --image us-central1-docker.pkg.dev/[PROJECT_ID]/my-lab-repo/game2048:v1 \
    --region us-central1 \
    --allow-unauthenticated \
    --port 80
Use code with caution.
Parameters Explained:
--allow-unauthenticated: Makes the service public so anyone can access it via a URL.
--port 80: Tells Cloud Run that the application inside the container is listening on port 80.
🔗 Accessing the App
Once the deployment finishes, the terminal will provide a Service URL.
Example: https://run.app
💡 Summary of Workflow
Repository: Created the storage space.
Auth: Linked local Docker to GCP.
Tag: Labeled the image for the destination.
Push: Uploaded the files.
Deploy: Launched the web service.
Do you want to add a "Cleanup" section to the README to show how to delete these resources and avoid costs?




