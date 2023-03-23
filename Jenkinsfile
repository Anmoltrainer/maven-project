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

            stage("Build Docker Image"){
                steps {
                  script{
                      sh 'sudo su -'
                      sh 'docker build -t cloudzenix/cz-bookstore .'
                    }
                }
            }
   
     }
}

