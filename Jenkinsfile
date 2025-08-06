 pipeline{
  agent any
  stages {
     stage('test-job') {
        steps {
            slackSend channel: 'new-channel', color: '#439FE0', message: 'Ecomm deployment job started', teamDomain: 'acmeco-wro7728', tokenCredentialId: 'slack'
            }
        } 
    stage ('web server') {
      steps {
        sh 'sudo apt install nginx -y'
      }
    }
    stage ('default page removel') {
      steps{
        sh 'sudo rm -rf /var/www/html/*'
      }
    }
    stage ('deploy') {
      parallel{
        stage ('Linux Deployment'){
          steps{
            sh 'sudo cp -rf /var/lib/jenkins/workspace/ecomm/* /var/www/html'
          }
        }
        stage ('Windows Deployment'){
          steps{
            echo 'wind deployment done'
          }
        }
     }
    }
    stage ('restart') {
      steps{
        sh 'sudo systemctl restart nginx'
      }
    }
    stage ('Testing') {
      parallel{
          stage('andraiod'){
            steps{
              echo "testing on andraiod Phone"
             }
          }
         stage('I Phone'){
            steps{
              echo "testing on Apple Phone"
             }
          }
          stage('Linux'){
            steps{
              echo "testing on Linux"
             }
          }
         stage('Windows'){
            steps{
              echo "testing on windows"
             }
          }
      }
    }
    stage('email-notification') {
       steps {
           emailext body: 'ecomm applications deployed successfully into prod environment', subject: 'ecomm deployment', to: 'devarajabeena111@gmail.com' 
            }
            
         }
  }
}
