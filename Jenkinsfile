pipeline{

    agent any

    environment{
        DOCKERHUB_CREDENTIALS = credentials('med-dockerhub')
    }

    stages{

        // stage('Git Checkout'){

        //     steps{
        //         git branch: 'main', url: 'https://github.com/MeD1100/Notification-microservice.git'
        //     }
        // }

        stage('Build du projet') {
            agent { 
                docker 'maven:3.8.3-openjdk-17' 
            }

            steps {
                // withMaven(maven:'3.8.3') {
                //     sh 'mvn install -DskipTests '
                // }
                
				stash includes: 'notification-service/target/*.jar', name: 'targetfiles'
            }
        }

        stage('Construction image') {
            steps {
                 unstash 'targetfiles'
			   script {
                        sh 'docker build ./notification-service -t notification-service-via-jenkins:latest'
                        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
						sh 'docker tag notification-service-via-jenkins medrh/notification-service-via-jenkins'
						sh 'docker push medrh/notification-service-via-jenkins'
                }
            }
        }


    }
    
    post{
        always{
            sh 'docker logout'
        }
    }
}