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
      stage('Parallel Stages') {
            parallel {
                stage('Restart Nginx') {
                    steps {
                        script {
                            // Restart Nginx
                            sh 'sudo systemctl restart nginx'
                            echo 'Deployment is completed'
                        }
                    }
                }
                stage('Hosting') {
                    steps {
                        script {
                            def get_public_ip() {
                                // Use curl to query ifconfig.co for the public IP address
                                public_ip = sh(script: "curl -s https://api.ipify.org", returnStdout: true).trim()
                                return public_ip
                            }

                            // Retrieve the public IP address using a web service
                            def public_ip = get_public_ip()

                            def httpResponse = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://${public_ip}", returnStdout: true).trim()
                            if (httpResponse != '200') {
                                // If HTTP status code is not 200, retry the hosting stage
                                echo "HTTP status code is not 200. Retrying hosting stage."
                                retry(3) {
                                    sh 'sudo cp -rf /var/lib/jenkins/workspace/ecomm-app/* /var/www/html/'
                                }
                            } else {
                                // HTTP status code is 200, proceed with hosting
                                sh 'sudo cp -rf /var/lib/jenkins/workspace/ecomm-app/* /var/www/html/'
                            }
                        }
                    }
                }
            }
        }
    }  
}
