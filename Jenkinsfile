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
            stage("Deploy to Tomcat server") {
                steps{
                  deploy adapters: [tomcat9(credentialsId: 'TomcatloginID', path: '', url: 'http://18.207.141.184:8080/')], contextPath: '/opt/tomcat/webapps', war: '**/*.war'
                }
            }
        }
}

