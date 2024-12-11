pipeline {
    agent any
    environment {
        IMAGE_NAME = 'antonios94/jenkins-flask-app'
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
        KUBECONFIG = credentials('kubeconfig')

    }
    
    stages {
        stage('Build Docker Image')
        {
            steps
            {
                sh "podman build -t ${IMAGE_TAG} ."
                echo "Docker image build successfully"
                sh "podman images"
            }
        }
        stage('Login to docker hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub',usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]) {
                sh "podman login -u ${USERNAME} -p ${PASSWORD} docker.io" }
                echo 'Login successfully'
            }
        }
        stage('Push Docker image') {
            steps {
		        sh "podman push ${IMAGE_TAG}"
                echo "Docker image push successfully"
            }
        }
        
        
        stage('Deploy to k8s Cluster') {
            steps {
                sh "kubectl apply -f deployment.yaml"
                echo "Deployed to k8s Cluster"
            }
        }
    }
}
