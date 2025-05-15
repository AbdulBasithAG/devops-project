pipeline {
    agent any
    environment {
        IMAGE_NAME = "your-dockerhub-username/devops-app:${BUILD_NUMBER}"
    }
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/your-username/devops-project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    docker push $IMAGE_NAME
                    '''
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh '''
                sed "s|IMAGE|$IMAGE_NAME|" k8s/deployment.yaml > k8s/temp-deploy.yaml
                kubectl apply -f k8s/temp-deploy.yaml
                '''
            }
        }
    }
}
