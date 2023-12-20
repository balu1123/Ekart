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
            withMaven(globalMavenSettingsConfig: 'Global-settings.xml') {
     
            sh 'mvn deploy -DskipTests=true'   
              }
            
          }  
        }

        stage("Build & Tag Docker"){
           steps{
             script{
                withDockerRegistry(credentialsId: 'Docker-cred') {
                sh 'sudo chmod 666 /var/run/docker.sock'    
                sh "docker build -t shopping-cart:dev -f docker/Dockerfile ."  
                sh "docker tag shopping-cart:dev bbaludevops/shopping-cart:dev" 
                }
             }
           } 
        }
    }
}       
