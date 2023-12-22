pipeline {
    agent any
    tools{
        
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
            nexusArtifactUploader artifacts: [
                [artifactId: 'shopping-cart',
                 classifier: '', 
                 file: '', 
                 type: 'jar']
                 ], 
                 credentialsId: 'nexus-cred', 
                 groupId: 'com.reljicd', 
                 nexusUrl: '54.162.221.203:8081', 
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: 'ekart-SNAPSHOT', 
                 version: '2.0.0'
            withMaven(globalMavenSettingsConfig: 'global-settings.xml') {
            sh "mvn clean deploy -DskipTests=true" 
            }
          }  
        }
        
    }
}       
