pipeline {
  agent any
  tools {
    maven 'Maven_3.2.5'
  }
  stages {
    stage('Compile and Run Sonar Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]){
          sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggyappasecguru -Dsonar.organization=buggyappasecguru -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=${SONAR_TOKEN}'
        }
      }
    }

    stage('SCA Analysis with SNYK') {
      steps {		
        withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
          sh 'mvn snyk:test -fn'
        }
      }
    }

    stage('Build Images') {
      steps {
        script {
          docker.build("asg")
        }
      }
    }

    stage('Push Images') {
      steps {
        script {
          docker.withRegistry('https://336953435369.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
            docker.image("asg").push()
            /* app.push("latest") */
          }
        }
      }
    }

    /*
    stage('Kubernetes Deployment') {
      steps {
        withKubeConfig([credentialsId: 'kubelogin']) {
          sh('kubectl delete all --all -n devsecops')
          sh ('kubectl apply -f deployment.yml --namespace=devsecops')
        }
      }
    }

    stage ('wait_for_testing') {
      steps {
        sh 'pwd; sleep 180; echo "Application Has been deployed on K8S"'
      }
    }

    stage('Run DAST with ZAP') {
      steps {
        withKubeConfig([credentialsId: 'kubelogin']) {
          sh('zap.sh -cmd -quickurl http://$(kubectl get services/asgbuggy --namespace=devsecops -o json| jq -r ".status.loadBalancer.ingress[] | .hostname") -quickprogress -quickout ${WORKSPACE}/zap_report.html')
          archiveArtifacts artifacts: 'zap_report.html'
        }
      }
    }
    */
  }
}
