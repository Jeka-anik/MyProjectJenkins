
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
                sh "ansible-playbook -i ec2.py tag_Owner_Anik -m ping -u ubuntu --private-key=hw41.pem"
            }
        } 
       stage("Ansible") {
            steps {
               ansiblePlaybook become: true, becomeUser: 'ubuntu', credentialsId: 'hw41', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'ec2.py', playbook: 'deploy.yml'
            }              
       }
    }
}
