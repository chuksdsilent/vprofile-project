pipeline {
    agent any
    environment {
        registryCredential = 'ecr:us-east-1:aws-creds'
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
                  docker.withRegistry( vprofileRegistry, registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                  }
             }
        }
    }

    stage('Upload App Image') {
          steps{
              script{
                  docker.withRegistry( vprofileRegistry, registryCredential ) {
                    dockerImage.push("$BUILD_NUMBER")
                  }
              }
          }
     }
  }
}
