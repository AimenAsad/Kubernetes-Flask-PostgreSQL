🚀 Kubernetes Deployment: Flask App with PostgreSQL
This project demonstrates the deployment of a Flask-based web application with a PostgreSQL database on a Minikube-managed Kubernetes cluster. It covers containerization, service configuration, and persistent data storage within a Kubernetes environment.

🎯 Objective
The primary objective of this task was to gain hands-on experience with deploying a multi-component application on Kubernetes. This included:

Setting up and managing a local Kubernetes cluster using Minikube.

Deploying a PostgreSQL database with a persistent volume to ensure data durability.

Containerizing and deploying a Flask web application configured to connect to the PostgreSQL database.

Configuring Kubernetes Services to expose both the database and the Flask application within the cluster.

Performing testing and verification to confirm application accessibility and database connectivity.

🛠️ Technologies Used
Kubernetes (Minikube): For local cluster management and container orchestration.

kubectl: The command-line tool for interacting with Kubernetes clusters.

Docker: For containerizing the Flask application.

Flask: Python web framework for the application.

PostgreSQL: Relational database for data storage.

Python: Programming language for the Flask application.

📂 Project Structure
k8s-flask-app/
│── manifests/
│   │── deployment/
│   │   ├── flask-deployment.yaml     # Kubernetes Deployment for Flask app
│   │   └── postgres-deployment.yaml  # Kubernetes Deployment for PostgreSQL
│   │── service/
│   │   ├── flask-service.yaml        # Kubernetes Service for Flask app (NodePort/LoadBalancer)
│   │   └── postgres-service.yaml     # Kubernetes Service for PostgreSQL (ClusterIP)
│   │── configmap/
│   │   └── postgres-configmap.yaml   # Kubernetes ConfigMap for PostgreSQL configuration
│   │── secret/
│   │   └── postgres-secret.yaml      # Kubernetes Secret for PostgreSQL credentials
│── app/
│   │── Dockerfile                    # Dockerfile for the Flask application
│   │── requirements.txt              # Python dependencies for the Flask app
│   │── app.py                        # Flask application code
│── README.md
│── submission/                       # Folder for required submission snapshots
    ├── internal_deployment_snapshot.png
    ├── kubectl_get_all_snapshot.png
    ├── scaling_effect_snapshot.png
    └── min_max_replicas_investigation.md
🚀 Deployment Guide
1. Setup Minikube
Ensure Minikube is installed and running. If not, start your Minikube cluster:

Bash

minikube start
2. Deploy PostgreSQL Database
First, deploy the PostgreSQL database components, including its Deployment, Service, ConfigMap, and Secret.

Create Secret:
Edit manifests/secret/postgres-secret.yaml with your base64 encoded PostgreSQL password.
(To base64 encode your password: echo -n 'your_password' | base64)
Apply the secret:

Bash

kubectl apply -f manifests/secret/postgres-secret.yaml
Create ConfigMap:
Apply the ConfigMap for PostgreSQL configuration:

Bash

kubectl apply -f manifests/configmap/postgres-configmap.yaml
Deploy PostgreSQL:
Apply the PostgreSQL deployment and service manifests:

Bash

kubectl apply -f manifests/deployment/postgres-deployment.yaml
kubectl apply -f manifests/service/postgres-service.yaml
3. Deploy Flask Application
Before deploying the Flask app, you need to containerize it and push the image to a Docker registry (e.g., Docker Hub, or use Minikube's Docker daemon).

Build Flask Docker Image:
Navigate to the app/ directory and build your Docker image. Replace your_docker_username/flask-app with your desired image name and tag.

Bash

cd app
docker build -t your_docker_username/flask-app:latest .
Push Docker Image (if using external registry):

Bash

docker push your_docker_username/flask-app:latest
If using Minikube's Docker daemon:

Bash

eval $(minikube -p minikube docker-env)
docker build -t flask-app:latest .
(Remember to revert Docker env after building for minikube: eval $(minikube -p minikube docker-env -u) if you switch back to local docker)

Update Flask Deployment:
Edit manifests/deployment/flask-deployment.yaml to ensure the image field points to your newly built Docker image (e.g., your_docker_username/flask-app:latest or flask-app:latest if using minikube's docker daemon).

Deploy Flask Application:
Apply the Flask deployment and service manifests:

Bash

kubectl apply -f manifests/deployment/flask-deployment.yaml
kubectl apply -f manifests/service/flask-service.yaml
4. Testing & Verification
Check Pods and Services Status:
Verify that all deployments and services are running as expected:

Bash

kubectl get all
(Replace this with a screenshot of your terminal showing the output of kubectl get all)

Access the Flask Application:
The Flask application is exposed via a NodePort service. Find the URL to access it:

Bash

minikube service flask-service --url
Open the provided URL in your web browser. This should show your Flask application's UI. Test its functionalities (e.g., adding data) to confirm database connectivity to PostgreSQL.

📸 Submission Requirements
The submission/ folder in this repository contains the following:

Snapshot of the internal deployment: A screenshot showing the state of pods, deployments, and services within the cluster (e.g., kubectl get all output).

Snap of the 'kubectl get all': (Already covered above for clarity).

Test the effect of scaling up and down the replica set: Documented observations or screenshots showing the number of pods changing after scaling commands (e.g., kubectl scale deployment/flask-deployment --replicas=3).

Investigate the min and max replicas count in the deployment file: A brief explanation or markdown file (min_max_replicas_investigation.md) discussing the minReplicas and maxReplicas settings (often used with Horizontal Pod Autoscaler, but can refer to initial replicas count and potential HPA configurations if implemented).

