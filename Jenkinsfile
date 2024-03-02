pipeline {
  agent any
  tools {
    maven 'Maven_3.2.5'
  }
  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggyappasecguru -Dsonar.organization=buggyappasecguru -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=bef0ed1dead9ab7cd394036fd38a472ae5eec454'
      }
    }
    stage('Build') {
      steps {
        script {
          docker.build("asg")
        }
      }
    }
    stage('Push') {
      steps {
        script {
          docker.withRegistry('https://336953435369.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
            app.push("latest")
          }
        }
      }
    }
  }
}
