pipeline {
    agent any

    tools {
        maven 'Maven'
    }

    stages {

        stage('CompileandRunSonarAnalysis') {
            steps {
                sh '''
                mvn clean verify sonar:sonar \
                -Dsonar.projectKey=secguru123 \
                -Dsonar.organization=secguru123 \
                -Dsonar.host.url=https://sonarcloud.io \
                -Dsonar.token=784a5611f0fc696b3415fd3d335b5be954921726
                '''
            }
        }

        stage('Snyk SCA Scan') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    sh '''
                        snyk auth $SNYK_TOKEN
                        snyk test
                    '''
                }
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

