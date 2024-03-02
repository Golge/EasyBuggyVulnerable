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
            docker.image("asg").push()
            /* app.push("latest") */
          }
        }
      }
    }
    stage('Kubernetes Deployment of ASG Bugg Web Application') {
	    steps {
	        withKubeConfig([credentialsId: 'kubelogin']) {
		        sh('kubectl delete all --all -n devsecops')
		        sh ('kubectl apply -f deployment.yml --namespace=devsecops')
		        }
	    }
   	}
  	stage ('wait_for_testing'){
	    steps {
		    sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
	  	}
	  }
	   
	  stage('RunDASTUsingZAP') {
      steps {
		    withKubeConfig([credentialsId: 'kubelogin']) {
				  sh('zap.sh -cmd -quickurl http://$(kubectl get services/asgbuggy --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
				  archiveArtifacts artifacts: 'zap_report.html'
		    }
	    }
    }
  }
}
