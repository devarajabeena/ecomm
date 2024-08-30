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
  }
}
