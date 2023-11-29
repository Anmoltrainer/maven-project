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
                    sh 'docker tag bprasad701/obs-java:latest 3.227.229.102:8082/bhanu-docker-images/obs-java:latest'
                    sh 'docker login -u admin -p Vbp1993@gvr 3.227.229.102:8082'
                    sh 'docker push 3.227.229.102:8082/bhanu-docker-images/obs-java:latest'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                        // Authenticate with EKS cluster
                        sh 'aws eks --region us-east-1 update-kubeconfig --name cz-eks-cluster'
                        sh 'echo "AWS CLI Configuration:"'
                        sh 'aws configure list'

                        sh 'echo "Kubectl Configuration:"'
                        sh 'kubectl config view'


                        
                        // Deploy to EKS cluster
                        // sh 'kubectl apply -f eks-deployment.yaml'
                }
            }
        }
    }
}
