
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
        
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
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
                sh "pwd"
                sh "./ec2.py --list"
                //sh "ansible -i ec2.py all -m ping"
                sh "ansible-playbook -i 'ec2.py' type_t3_micro deploy.yml"
            }
        } 
       stage("Ansible") {
            steps {
               ansiblePlaybook become: true, becomeUser: 'ubuntu', credentialsId: 'd2413b3b-07e6-4a40-842b-f15e1d6ed3e5', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'hw41', playbook: 'deploy.yml'
            }              
       }
    }
}
