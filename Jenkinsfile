pipeline {
    agent any

    environment {
        SONAR_TOKEN = 'sqp_1991cf1af85d111cd8cce4d8ddbd21f4053fbdb8'
        SONAR_PROJECT_KEY = 'open-cart-app'
        SONAR_PROJECT_NAME = 'open-cart app'
        SONAR_HOST_URL = 'http://your-sonarqube-server:9000' // replace with your SonarQube URL
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                git branch: 'master', url: 'https://github.com/bharath-ragipani/OpencartV121.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Running Maven clean and test...'
                bat 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withEnv(["SONAR_TOKEN=${env.SONAR_TOKEN}"]) {
                    bat """
                        mvn sonar:sonar ^
                        -Dsonar.projectKey=${env.SONAR_PROJECT_KEY} ^
                        -Dsonar.projectName="${env.SONAR_PROJECT_NAME}" ^
                        -Dsonar.host.url=${env.SONAR_HOST_URL} ^
                        -Dsonar.login=${env.SONAR_TOKEN}
                    """
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging project...'
                bat 'mvn clean package'
            }
        }

        stage('Archive Reports') {
            steps {
                echo 'Archiving test reports and artifacts...'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                archiveArtifacts artifacts: '**/target/surefire-reports/**', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'Build & Test completed successfully!'
        }
        failure {
            echo 'Build failed! Check logs for details.'
        }
    }
}
