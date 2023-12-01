pipeline {
    agent any
    tools{
        jdk  'jdk17'
        maven  'maven'
    }
    
    
    stages {
        stage("Git Checkout"){
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/balu1123/Ekart.git'
            }
        }

        stage("COMPILE"){
            steps {
                sh "mvn clean compile"
            }
        }

        stage("Build"){
            steps {
                sh "mvn clean package -DskipTests=true"
            }
        }

        stage("OWASP"){
          steps{
            dependencyCheck additionalArguments: '', odcInstallation: 'DP'
            dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
          }  
        }
        
        stage('Sonarqube') {
            steps {
                withSonarQubeEnv('sonar-scanner'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=ekart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=ekart '''
               }
            }
        }
    }
}       
