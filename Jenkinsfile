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

        
    }
}       
