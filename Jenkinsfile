pipeline {
  environment {
    registry = "883081664011.dkr.ecr.us-east-2.amazonaws.com/your_ecr_repo1"
    imagename = "kennethreitz/httpbin"
    registryCredential = 'kennethreitz'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/midhil18/httpbin', branch: 'patch-1', credentialsId: 'midhil18'])
 
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
                sh 'aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 883081664011.dkr.ecr.us-east-2.amazonaws.com'
                sh 'docker push 883081664011.dkr.ecr.us-east-2.amazonaws.com/repo_ecr1:latest'
          }
        }
    }
    stage('stop previous containers') {
      steps{
            sh 'docker ps -f name=httpbin_image -q | xargs --no-run-if-empty docker container stop'
            sh 'docker container ls -a -fname=httpbin_image -q | xargs -r docker container rm'
 
      }
    }
    stage('Docker Run') {
      steps{
         script {
                 sh 'docker run -d -p 8096:5000 --rm --name httpbin_image 883081664011.dkr.ecr.us-east-2.amazonaws.com/repo_ecr1'
                }
            }   
        }
  }
}
