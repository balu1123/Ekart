pipeline {
    agent any
    tools{
        jdk  'jdk17'
        maven  'maven'
    }
    
    environment{
        SCANNER_HOME= tool 'sonar-scanner'
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
        
        stage("Sonarqube") {
            steps {
                withSonarQubeEnv('sonar-scanner'){
                   sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=EKART \
                   -Dsonar.java.binaries=. \
                   -Dsonar.projectKey=EKART '''
               }
            }
        }

        stage("Nexus"){
          steps{
            script{
               nexusArtifactUploader artifacts: 
               [
                 [artifactId: 'spring-boot-starter-parent', 
                  classifier: '',
                  file: '', 
                  type: 'jar']
                  ], 
                  credentialsId: 'nexus-cred', 
                  groupId: 'org.springframework.boot', 
                  nexusUrl: '54.80.110.73:8081', 
                  nexusVersion: 'nexus3', 
                  protocol: 'http', 
                  repository: 'ekart-release', 
                  version: '1.5.3.RELEASE' 
            }
          }  
        }
    }
}       
