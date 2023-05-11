pipeline {
    agent any
    environment {
        registryCredential = 'ecr:us-east-1:aws-cred'
        appRegistry = "598478793993.dkr.ecr.us-east-1.amazonaws.com/vprofileappimg"
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
             sh 'docker build -t ${appRegistry}:${BUILD_NUMBER} ./Docker-files/app/multistage/'
             }

     }
    
    }

    stage('Upload App Image') {
          steps{
            script {
                sh 'docker push ${appRegistry}:${BUILD_NUMBER}'
            }
          }
     }
     
  }
}
