pipeline {
    agent any
    environment {
        registryCredential = 'ecr:us-east-1:aws-creds'
        appRegistry = "598478793993.dkr.ecr.us-east-1.amazonaws.com"
        IMG_NAME = "vprofileappimg"        
        vprofileRegistry = "https://598478793993.dkr.ecr.us-east-1.amazonaws.com"
        cluster = "vprofile"
        service = "vprofileappsvc"
    }
     tools {
        maven 'maven_3_5_0'
    }
  stages {
    stage('Fetch code'){
      steps {
        git branch: 'docker', url: 'https://github.com/devopshydclub/vprofile-project.git'
      }
    }
    stage('Test'){
      steps {
        sh 'mvn test'
      }
    }
      
    stage('Build App Image') {
       steps {
             script {
                 sh 'docker build -t awsdockerimg:${BUILD_NUMBER} ./Docker-files/app/multistage/'
                 sh 'docker tag awsdockerimg:${BUILD_NUMBER} ${appRegistry}/${IMG_NAME}:${BUILD_NUMBER}'
             }
        }
    }

    stage('login into aws ecr'){
      steps {
        sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${appRegistry}'
      }
    }
    stage('Upload App Image') {
          steps{
              script{
                  sh 'docker push  ${appRegistry}/${IMG_NAME}:${BUILD_NUMBER}'
              }
          }
     }
     
    stage('Upload App Image') {
          steps{
              withAWS(credentials: 'awss-creds', region: 'us-east-1'){
                  sh 'aws ecs update-service --cluster ${cluster} --service ${service
 --forece-new-deployment
          }
     }
  }
}
