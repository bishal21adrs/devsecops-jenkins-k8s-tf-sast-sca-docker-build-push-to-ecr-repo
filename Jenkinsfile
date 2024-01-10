pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=awsdevsecops1_aws -Dsonar.organization=awsdevsecops1 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=f6e90ab78048b186236b73b037b406b60e52f623'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }	

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("awsd")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://975049920961.dkr.ecr.ap-southeast-2.amazonaws.com', 'ecr:ap-southeast-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
