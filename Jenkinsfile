
pipeline {
    agent any
    environment {
        imagename = "jekanik/projectfordiplom"
        registryCredential = 'git'
        dockerImage = ''
        CLASS           = "GitSCM"
        BRANCH          = "main"
        GIT_CREDENTIALS = "git-hubsshkey"
        GIT_URL         = "git@github.com:Jeka-anik/MyProjectCode.git"
    }
    stages {
        stage('Hello') {
            steps {
                echo 'Hello World, from Anik!'
            }
        }
      stage('Checkout SCM') {
            steps {
                checkout([
                    $class: "${CLASS}",
                    branches: [[name: "${BRANCH}"]],
                    userRemoteConfigs: [[
                    url: "${GIT_URL}",
                    credentialsId: "${GIT_CREDENTIALS}",
                    ]]
                ])
            }
        }
       stage("Prepare build image") {
            steps {
                sh "sudo docker build -f Dockerfile . -t jekanik/projectfordiplom:${BUILD_ID}"
                sh "docker login -u jekanik -p${password}"
                sh "docker push jekanik/projectfordiplom:${BUILD_ID}"
            }
        }      
        
    }
}
