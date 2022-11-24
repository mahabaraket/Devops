pipeline {
  agent any
  tools {
    
     jdk 'JAVA_HOME'
     maven 'M2_HOME'
    
  }
  stages {
    
    stage ('CHECK TOOLS') {
            steps {
                sh 'java --version'
                sh 'mvn --version'
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
    stage('CLEANING') {
      steps {
         sh 'echo "Cleaning is processing ...."'
        sh 'mvn clean'
      }
    }
    
    stage ('ARTIFACT') {
            steps {
                sh 'echo "Artifact construction is processing ...."'
                sh 'mvn -DskipTests package'
              
            }
            
        }
    stage('JUNIT TEST') {
      steps {
         sh 'echo "Junit Test is processing ...."'
        sh 'mvn  test'

      }
    }
	  
     stage('SONARQUBE') {
		        steps {
		        withSonarQubeEnv('Sonarqube') {
		        sh 'mvn clean -DskipTests package sonar:sonar'
	                  }
	                }
	            }
	stage("NEXUS") {
			steps {
				sh 'mvn clean deploy -DskipTests'
          }
        }
	  
    stage('DOCKER build image') {
      steps {
         sh 'echo "Docker build image is processing ...."'
        sh 'docker build -t heladarguechi/achat .'

      }
    }
	  
     stage('DOCKER login') {
      steps {
         sh 'echo "Docker login is processing ...."'
        sh 'docker login --username heladarguechi --password 98993295Azerty!'

      }
    }
	  
    stage('DOCKER push') {
      steps {
         sh 'echo "Docker push is processing ...."'
        sh 'docker push heladarguechi/achat:latest'

      }
    }

	  
    stage('DOCKER-COMPOSE') {
      		steps {
         		sh 'docker-compose up -d'
      }
    }
	  
  }
  post {
    success { mail to: "darguechihela@gmail.com",
                    subject: "Build sucess",
                    body: "sucess, Great work Hela!"
             echo 'successful'}
    failure { mail to: "darguechihela@gmail.com",
                    subject: "Build failed",
                    body: "failed, check your work you have an error!"
             echo 'failed'}
  }
}



