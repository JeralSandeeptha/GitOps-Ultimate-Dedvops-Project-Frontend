pipeline {
    agent any;

    environment {
        APP_NAME = "ultimate-devops-project-frontend"
        GIT_TOKEN = 'jeral'
        GIT_USER = 'JeralSandeeptha'
        REPO = 'GitOps-Ultimate-Dedvops-Project-Frontend'  
        DOCKER_USER = "jeralsandeeptha"
        DOCKER_PASS = 'dockerhub'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    }

    stages {
      
        stage('Cleanup the Workspace') {
            steps {
                cleanWs();
            }
        }

        stage('Checkout from SCM') {
          steps {
            git branch: 'main', url: 'https://github.com/JeralSandeeptha/GitOps-Ultimate-Dedvops-Project-Frontend.git'
          }
        }

        stage('Update the Deployment Tags') {
          steps {
              powershell """
                  Get-Content deployment.yaml
                  (Get-Content deployment.yaml) -replace '${APP_NAME}:.*', '${APP_NAME}:${IMAGE_NAME}' | Set-Content deployment.yaml
                  Get-Content deployment.yaml
              """
          }
       } 

       stage('Push the changed deployment file to Git') {
            steps {
                withCredentials([string(credentialsId: 'GITHUB_TOKEN', variable: 'GIT_TOKEN')]) {
                    bat """
                    git config --global user.name "${env.GIT_USER}"
                    git config --global user.email "jeral.sandeeptha1@gmail.com"
                    git remote set-url origin https://${env.GIT_USER}:%GIT_TOKEN%@github.com/${env.GIT_USER}/${env.REPO}.git
                    git add deployment.yaml
                    git commit -m "Updated deployment manifest" || echo "Nothing to commit"
                    git push origin main
                    """
                }
            }
        }
    }
}
