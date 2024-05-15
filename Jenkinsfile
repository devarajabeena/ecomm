pipeline {
  agent any
    stages{
      stage('slack-notification'){
        steps{
          slackSend channel: 'jenkins-notification', color: '439FE0', message: 'slackSend "started ${JOB_NAME} ${BUILD_NUMBER} (<${BUILD_URL}|Open>)', teamDomain: 'dl-muraliworkspace', tokenCredentialId: 'slack', username: 'jenkins'
        }
      }
      stage ('install nginx'){
        steps{
            sh 'sudo apt install nginx -y'          
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
