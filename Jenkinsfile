pipeline{

    agent any

    environment{
        DOCKERHUB_CREDENTIALS = credentials('med-dockerhub')
    }

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
                
				stash includes: 'notification-service/target/*.jar', name: 'targetfiles'
            }
        }

        // stage('Construction image') {
        //     steps {
        //          unstash 'targetfiles'
		// 	   script {
        //                 sh 'docker build ./notification-service -t notification-service-via-jenkins:latest'
        //                 sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
		// 				sh 'docker tag notification-service-via-jenkins medrh/notification-service-via-jenkins'
		// 				sh 'docker push medrh/notification-service-via-jenkins'
        //         }
        //     }
        // }

    //     stage('Deploying to Kubernetes') {
    //         steps {
    //             withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'KubernetesByAdmin', namespace: 'default', serverUrl: 'https://xxx:yyyy']]) {
    //                  sh 'curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.20.5/bin/linux/amd64/kubectl"'
    //                  sh 'chmod u+x ./kubectl'
    //                  sh './kubectl apply -f notification-service/notification-service-deployment.yaml'
    //             }
    //   }

            stage('Uploading jar files to Nexus'){
                steps{

                    nexusArtifactUploader artifacts:
                    [
                        [
                            artifactId: 'notification-service',
                            classifier: '',
                            file: 'target/notification-service-1.0-SNAPSHOT.jar',
                            type: 'jar'
                        ]
                    ],
                    credentialsId: 'nexus-auth',
                    groupId: 'com.programming.techie',
                    nexusUrl: 'localhost:8110',
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    repository: 'sofia-app-release',
                    version: '1.0-SNAPSHOT'
                }
            }


    }

    // post{
    //     always{
    //         sh 'docker logout'
    //     }
    // }
}