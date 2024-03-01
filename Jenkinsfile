pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggyappasecguru -Dsonar.organization=buggyappasecguru -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=f50fc28a614de00f1b5c5d1ae8443ecdf6b057e8'
			}
        } 
  }
}
