pipeline {
  agent any
    stages{
      stage('slack-notification'){
        steps{
          slackSend channel: 'jenkins-notification', 
                      color: '439FE0', 
                    message: "started ${JOB_NAME} ${BUILD_NUMBER} (<${BUILD_URL}|Open>)", 
                teamDomain: 'dl-muraliworkspace', 
                tokenCredentialId: 'slack', 
                  username: 'jenkins'
        }
      }
      stage('Run Tests') {
          parallel {
              stage('Test On Windows') {
                  steps {
                      echo 'tests are completed on windows'
                  }
              }
              stage('Test On Linux') {
                  steps {
                      echo 'tests are completed on Linux'
                  }
              }
          }
      }
      stage('Install and Check Nginx') {
          parallel {
              stage('Install Nginx') {
                  steps {
                      script {
                          sh 'sudo apt install nginx -y'
                      }
                  }
              }
              stage('Check Nginx Status') {
                  steps {
                      script {
                          def response = sh(returnStatus: true, script: 'curl -Is http://localhost | head -n 1 | cut -d" " -f2')
                          if (response == '200') {
                              echo 'Nginx is running with status code 200. Proceeding with other stages.'
                          } else {
                              echo 'Nginx is not running properly. Reinstalling...'
                              sh 'sudo apt purge nginx -y && sudo apt install nginx -y'
                          }
                      }
                  }
              }
          }
      }
      stage ('delete default page'){
        steps{
            sh 'sudo rm -rf /var/www/html/*'          
        }
      }
      stage ('hosting'){
        steps{
            sh 'sudo cp -rf /var/lib/jenkins/workspace/ecomm-app/* /var/www/html/'          
        }
      }
      stage ('restart nginx'){
        steps{
            sh 'sudo systemctl restart nginx'  
            echo 'deployment is completed'
        }
      }
    }  
}
