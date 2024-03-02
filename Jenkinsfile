pipeline {
  agent any
  tools {
    maven 'Maven_3.2.5'
  }
  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggyappasecguru -Dsonar.organization=buggyappasecguru -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=9325586a8f1d1adf470b908a46156f5844'
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
