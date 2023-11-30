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
                sh "mvn clean compile -DskipTests"
            }
        }

        stage("OWASP Scan"){
            steps {
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage("Sonarqube"){
            steps {
                withSonarQubeEnv('sonar-server'){
                   sh ''' $SCANNER_HOME/bin/sonar
                   -Dsonar.projectName=ekart \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=ekart '''
                }
            }
        }                   
    }
}       
