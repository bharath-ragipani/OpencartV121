pipeline {
    agent any

    environment {
        // Set your JDK and Maven paths if needed
        JAVA_HOME = 'C:\\Program Files\\java21\\jdk-21.0.9'
        PATH = "${env.JAVA_HOME}\\bin;C:\\apache-maven-3.9.3\\bin;${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                git branch: 'main', url: 'https://github.com/your-username/your-new-project.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Running Maven clean and test...'
                bat 'mvn clean test'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the application...'
                bat 'mvn clean package'
            }
        }

        stage('Archive Artifacts') {
            steps {
                echo 'Archiving artifacts...'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('SonarQube Scan') {
            when {
                expression { false } // Change to true if you configure SonarQube
            }
            steps {
                echo 'Running SonarQube scan...'
                bat 'mvn sonar:sonar'
            }
        }
    }

    post {
        success {
            echo 'Build & Test successful!'
        }
        failure {
            echo 'Build failed. Check logs!'
        }
    }
}
