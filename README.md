
Experiment 19: Jenkins and Docker Integration 
Tasks: Build Docker image using Jenkins. 
Outcome: CI/CD integration skills.
give full steps and procodeur

Here’s a clean, step-by-step procedure (with commands + Jenkinsfile) for your experiment:

🧪 Experiment 19: Jenkins and Docker Integration
🎯 Objective
Build a Docker image automatically using Jenkins (CI pipeline).

🏗️ Architecture Flow
Developer pushes code → GitHub

Jenkins pulls code

Jenkins builds Docker image

(Optional) Push image to Docker Hub

⚙️ Prerequisites
Make sure installed:

Java (JDK 8/11+)

Jenkins

Docker

Git

👉 Verify:

java -version
docker --version
git --version
🔧 Step 1: Install Required Jenkins Plugins
Go to:
Jenkins Dashboard → Manage Jenkins → Plugins

Install:

Docker Pipeline

Git

Pipeline

Restart Jenkins.

🐳 Step 2: Configure Docker in Jenkins
Add Jenkins user to Docker group
On Linux:

sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
On Windows:

Docker Desktop must be running

Ensure Jenkins service can access Docker

📁 Step 3: Create Sample Project
Create a folder:

docker-jenkins-demo/
│
├── Dockerfile
├── app.java (optional)
└── Jenkinsfile
📦 Step 4: Create Dockerfile
FROM openjdk:11
WORKDIR /app
COPY . /app
RUN javac app.java
CMD ["java", "app"]
💻 Step 5: Sample Java Program
public class app {
    public static void main(String[] args) {
        System.out.println("Hello from Docker + Jenkins!");
    }
}
🔄 Step 6: Push Code to GitHub
git init
git add .
git commit -m "Initial commit"
git remote add origin <your-repo-url>
git push -u origin main
🔧 Step 7: Create Jenkins Pipeline Job
Go to Jenkins Dashboard

Click New Item

Enter name → docker-pipeline

Select Pipeline

Click OK

📜 Step 8: Add Jenkins Pipeline Script
In Pipeline section → choose:
👉 “Pipeline script from SCM”

SCM: Git

Repo URL: <your GitHub repo>

Script Path: Jenkinsfile

🧾 Step 9: Jenkinsfile (Core Part)
pipeline {
    agent any

    environment {
        IMAGE_NAME = "yourdockerhubusername/demo-app"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: '<your-repo-url>'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}").run("-d -p 8081:8080")
                }
            }
        }

        // Optional
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-credentials') {
                        docker.image("${IMAGE_NAME}").push("latest")
                    }
                }
            }
        }
    }
}
🔐 Step 10: Add Docker Hub Credentials (Optional)
Go to Jenkins → Manage Jenkins → Credentials

Add:

Username

Password

ID: docker-hub-credentials

▶️ Step 11: Build Pipeline
Click:
👉 Build Now

📊 Expected Output
✔ Jenkins will:

Clone repo

Build Docker image

Run container

Push image (optional)

🧪 Verify Docker Image
docker images
🌐 Verify Running Container
docker ps
Open browser:

http://localhost:8081
❗ Common Errors & Fix
1. Docker not found
docker: command not found
✔ Add Docker to PATH or install properly

2. Permission denied
Got permission denied while trying to connect to Docker daemon
✔ Run:

sudo usermod -aG docker jenkins
3. Jenkins build fails
✔ Check logs:

Console Output → Error details
🧾 Final Outcome
✅ You achieved:

CI pipeline using Jenkins

Automated Docker image build

Basic CI/CD integration

If you want, I can also give you:
✅ Viva questions
✅ One-page PDF notes
✅ Diagram for exam writing
✅ Windows-specific steps (since you’re using Windows)

Just tell me 👍
