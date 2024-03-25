pipeline {
  agent any

  tools {
    maven 'M2_HOME'
    
    }
  
  stages {
   stage('CheckOut') {
      steps {
        echo 'Checkout the source code from GitHub'
        git branch: 'main', url: 'https://github.com/roshanscgithub/Banking-project.git'
            }
    }
    
    stage('Package the Application') {
      steps {
        echo " Packaing the Application"
        sh 'mvn clean package'
            }
    }
    
    stage('Publish TestNG Reports using HTML') {
      steps {
      publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/root/.jenkins/workspace/Banking-project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
    }
    
    stage('Docker Image Creation') {
      steps {
        sh 'docker build -t roshdockerhub/banking-project:1.0' 
            }
    }
    stage('DockerLogin') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'Dockerlogin', passwordVariable: 'dockerhub-pass', usernameVariable: 'dockerhub-user')]) {
        sh 'docker login -u ${env.dockerhub-user} -p ${env.docker-pass}'
            }
        }
    } 
  
    stage('Push Image to DockerHub') {
      steps {
        sh 'docker push roshdockerhub/banking-project:1.0'
            }
    } 
        stage ('Configure Test-server with Terraform, Ansible and then Deploying'){
            steps {
                dir('my-serverfiles'){
                sh 'sudo chmod 600 BabucKeypair.pem'
                sh 'terraform init'
                sh 'terraform validate'
                sh 'terraform apply --auto-approve'
                }
            }
        }
        stage ('Deploy into test-server using Ansible') {
           steps {
             ansiblePlaybook credentialsId: 'BabucKeypair', disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'finance-playbook.yml'
           }
               }*/
     }
 post{
        success{
            slackSend( channel: "#27-apr-devops", token: "slack-authn", color: "good", message: "Test Email")
        }
    }
}
  
