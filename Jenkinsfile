pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9002'
        SONAR_AUTH_TOKEN = credentials('sonar-groceryshopping-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Nishath-Syed/GroceryShopping.git'
            }
        }

        stage('Build') {
            steps {
                // Use bat for Windows
                bat 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube_GroceryShopping_Server') {
                    bat 'mvn sonar:sonar'
                }
            }
        }

        /*stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }*/

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
