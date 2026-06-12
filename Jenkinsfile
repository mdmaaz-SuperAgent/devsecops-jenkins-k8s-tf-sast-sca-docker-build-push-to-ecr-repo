pipeline {
  agent any
  tools { 
        maven 'Maven'  
    }
    stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=secguru123 -Dsonar.organization=secguru123 -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=958058fac36d22a25096e9ae5a285c04298bdcdf'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				 withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        snyk auth $SNYK_TOKEN
                        snyk test
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Push') {
            steps {
                script{
                    docker.withRegistry('154807207531.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
