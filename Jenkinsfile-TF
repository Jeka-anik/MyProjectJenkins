pipeline {

    parameters {
        string(name: 'environment', defaultValue: 'terraform', description: 'Workspace/environment file to use for deployment')
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')

    }


     environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

   agent  any
    
        tools {
        "org.jenkinsci.plugins.terraform.TerraformInstallation" "terraform10210"
              }
    //    options {
    //            timestamps ()
    //            ansiColor('xterm')
    //        }
    stages {
        //stage('checkout') {
        //    steps {
         //        script{
        //                dir("terraform")
        //                {
        //                    git "https://github.com/Jeka-anik/ForJenkins.git"
        //                }
        //            }
        //        }
        //    }
        stage('Init') {
            steps {
                sh 'terraform init -input=true'
                  }
        }
        stage('Create workspace') {
            steps {
                //sh 'terraform init -input=false'
                //sh 'terraform workspace new ${environment}'
                sh 'pwd; terraform workspace select ${environment}'
            }
        }
        stage('Plan') {
            steps {
                //sh 'terraform init -input=false'
                //sh 'terraform workspace new ${environment}'
                //sh 'pwd; terraform workspace select ${environment}'
                sh "pwd; terraform plan -input=false -out tfplan"
                sh "pwd; terraform show -no-color tfplan > tfplan.txt"
            }
        }
        stage('Approval') {
           when {
               not {
                   equals expected: true, actual: params.autoApprove
               }
           }

           steps {
               script {
                    def plan = readFile 'tfplan.txt'
                    input message: "Do you want to apply the plan?",
                    parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
               }
           }
       }

        stage('Apply') {
            steps {
                sh "pwd; terraform apply -input=false tfplan"
            }
        }
    }

  }
