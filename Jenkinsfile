pipeline{

    agent any

    stages{

        stage('Git Checkout'){

            steps{
                git branch: 'main', url: 'https://github.com/MeD1100/Notification-microservice.git'
            }
        }

        stage('Build du projet') {
            agent { 
                docker 'maven:3.8.3-openjdk-17' 
            }

            steps {
                withMaven(maven:'3.8.3') {
                    sh 'mvn install -DskipTests '
                }
                
				stash includes: 'target/*.jar', name: 'targetfiles'
            }
        }
    }
}