pipeline {
    agent any

    environment {
        // Set Java and Maven paths if needed (update to your local installation paths)
        JAVA_HOME = 'C:\\Program Files\\java21\\jdk-21.0.9'
        MAVEN_HOME = 'C:\\apache-maven-3.9.3'
        PATH = "${env.JAVA_HOME}\\bin;${env.MAVEN_HOME}\\bin;${env.PATH}"
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
                // Run your TestNG tests via Maven
                bat 'mvn clean test'
            }
        }

        stage('Archive Reports') {
            steps {
                echo 'Archiving TestNG and other reports...'
                // Archive test reports
                archiveArtifacts artifacts: 'test-output/**/*.html', fingerprint: true
                archiveArtifacts artifacts: 'test-output/**/*.xml', fingerprint: true
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging project (optional if needed)...'
                bat 'mvn clean package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
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
