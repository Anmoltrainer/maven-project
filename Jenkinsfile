pipeline {
    agent any
    tools {
        maven 'Maven 3.9.0'
    }
        stages {
            stage("git_checkout") {
                    steps {
                    git branch: 'J2EE', credentialsId: 'githubloginID', url: 'https://github.com/bprasad701/onlinebookstore.git'
                    echo "repo cloned successfully"
                    }
                    }
            stage("Build") {
                steps {
                    sh 'mvn clean package'
                }
                post{
                    success{
                        echo "Archiving the artifacts"
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }

            stage("Build Docker Image") {
                steps {
                  script {
                      sh 'docker build -t cloudzenix/cz-bookstore .'
                    }
                }
            }

            stage("Push image to Dockerhub") {
               steps {
                 script {
                     withCredentials([string(credentialsId: 'dockerhub', variable: 'Password')]) {
                     sh 'docker login -u bprasad701 -p ${Password}'
                     }
                     sh 'docker image push cloudzenix/cz-bookstore:latest'
                   }
                }
            }
   
     }
}

