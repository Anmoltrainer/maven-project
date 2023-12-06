pipeline {
    agent any
    tools {
        maven 'Maven3'
    }

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
                    sh 'docker tag bprasad701/obs-java:latest 54.227.118.11:8082/bhanu-docker-images/obs-java:latest'
                    sh 'docker login -u admin -p Vbp1993@gvr 54.227.118.11:8082'
                    sh 'docker push 54.227.118.11:8082/bhanu-docker-images/obs-java:latest'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    // Use withCredentials to securely inject AWS credentials
                    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_cred', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                        sh 'aws eks update-kubeconfig --region us-east-1 --name CZ_EKS'
                        // Deploy to EKS cluster
                        sh '/home/ubuntu/bin/kubectl apply -f eks-deployment.yaml'
                }
            }
        }
    }
}
}
