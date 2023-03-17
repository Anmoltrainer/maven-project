pipeline {  
    agent any  
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
            }
        }
}
