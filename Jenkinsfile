pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://localhost:9002'
        SONAR_AUTH_TOKEN = credentials('groc-token')
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
        }*/

    stage('Package') {
            steps {
                // Since we ran 'package' in the Build stage, the WAR is already there.
                // We just archive it here.
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployment stage placeholder..."
                // bat "curl --upload-file target/BloodBank.war http://tomcat_user:tomcat_pass@localhost:8087/manager/text/deploy?path=/BloodBank&update=true"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
