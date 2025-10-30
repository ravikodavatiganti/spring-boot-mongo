pipeline {
    agent any

    tools {
        maven 'maven-3.9.9' // Matches Jenkins global tools name
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/kkdevopsb3/spring-boot-mongo-docker-kkfunda.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube') {
            steps {
                withSonarQubeEnv('Sonar') {
                    sh """
                    mvn sonar:sonar \\
                        -Dsonar.projectKey=spring-boot-mongo \\
                        -Dsonar.projectName='Spring Boot Mongo Project'
                    """
                }
            }
        }
   

        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'Docker') {
                        sh 'docker build -t ravikodavatiganti/mongospring:latest .'
                    }
                }
            }
        }
      

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'Docker') {
                        sh 'docker push ravikodavatiganti/mongospring:latest'
                    }
                }
            }
        }
    }
}
