pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Use Maven or Gradle for Java application build
                    sh 'mvn clean install'
                }
            }
        }

        stage('Dockerize') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t bprasad701/obs-java:latest .'
                }
            }
        }

        stage('Push to JFrog') {
            steps {
                script {
                    // Push Docker image to JFrog Artifactory
                    sh 'docker tag your-docker-repo/your-app:latest jfrog-artifactory-url/your-docker-repo/your-app:latest'
                    sh 'docker login -u your-jfrog-username -p your-jfrog-api-key jfrog-artifactory-url'
                    sh 'docker push jfrog-artifactory-url/your-docker-repo/your-app:latest'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    // Authenticate with EKS cluster
                    sh 'aws eks --region your-eks-region update-kubeconfig --name your-eks-cluster-name'

                    // Deploy to EKS cluster
                    sh 'kubectl apply -f your-kubernetes-deployment.yaml'
                }
            }
        }
    }
}
