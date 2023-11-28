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
                    sh 'docker tag bprasad701/obs-java:latest 3.234.139.255:8082/bhanu-docker-images/obs-java:latest'
                    sh 'docker login -u admin -p Vbp1993@gvr 3.234.139.255:8082'
                    sh 'docker push 3.234.139.255:8082/bhanu-docker-images/obs-java:latest'
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                script {
                    sh 'echo "AWS CLI Configuration:"'
                    sh 'aws configure list'
                    
                    // Configure AWS CLI with credentials
                    withAWS(credentials: 'aws-credentials-id') {
                        // Authenticate with EKS cluster
                        sh 'aws eks --region us-east-1 update-kubeconfig --name CZ-Cluster'
                        
                        // Configure kubectl with kubeconfig
                        //withKubeConfig(credentialsId: 'kubeconfig-credentials-id', serverUrl: 'https://your-cluster-api-server')
                            // Deploy to EKS cluster
                        sh 'kubectl apply -f eks-deployment.yaml'
                
                    }
                }
            }
        }
    }
}
