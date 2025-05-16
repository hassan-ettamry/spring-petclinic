pipeline {
    agent any

    tools {
        maven 'Maven 3.8.1'
    }

    environment {
        SONARQUBE = 'sonarqube_jinkins'
        SONAR_TOKEN = credentials('SonarQube_Jinkins')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/hassan-ettamry/spring-petclinic.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE}") {
                    sh "mvn sonar:sonar -Dsonar.projectKey=petclinic -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage("Quality Gate") {
           steps {
              timeout(time: 1, unit: 'MINUTES') {
              waitForQualityGate abortPipeline: true
             }
        }
      }
    }

    post {
        success {
            echo 'Build and SonarQube analysis succeeded!'
        }
        failure {
            echo 'Build or analysis failed! Check logs.'
        }
    }
}
