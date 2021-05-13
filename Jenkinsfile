
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
                sh "docker build -f Dockerfile . -t jekanik/projectfordiplom:${BUILD_ID}"
                sh "docker login -u jekanik -p${password}"
                sh "docker push jekanik/projectfordiplom:${BUILD_ID}"
            }
        } 
       stage("run ec2.py") {
            steps {
                sh "chmod +x ec2.py"
                sh "./ec2.py --list"
            }
        } 
       stage("Ansible") {
            steps {
                ansiblePlaybook colorized: true, credentialsId: 'd2413b3b-07e6-4a40-842b-f15e1d6ed3e5', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'hosts', playbook: 'deploy.yml'
            }
        }              
    }
}
