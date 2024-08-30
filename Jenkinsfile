pipeline{
  agent any
  stages{
    stage('install nginx'){
      steps{
        sh 'sudo apt install nginx -y'
        echo "nginx installed successfully"
      }
    }
    stage('remove default page'){
      steps{
        sh 'sudo rm -rf /var/www/html/*'
        echo "nginx webpage removed"
      }
    }
    stage('deploying ecomm app'){
      steps{
        sh 'sudo cp -rf /var/lib/jenkins/workspace/ecomm-job/* /var/www/html/'
        sh 'sudo systemctl restart nginx'
        echo "copied files to nginx root directory"
      }
    }
  }
}
