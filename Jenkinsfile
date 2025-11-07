pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\java21\\jdk-21.0.9'
        MAVEN_HOME = 'C:\\apache-maven-3.9.3'
        PATH = "${env.JAVA_HOME}\\bin;${env.MAVEN_HOME}\\bin;${env.PATH}"
        SONAR_TOKEN = credentials('sonar-token') // Add your SonarQube token in Jenkins credentials
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
                echo 'Running SonarQube scan...'
                bat """
                    mvn sonar:sonar ^
                    -Dsonar.projectKey=OpencartV121 ^
                    -Dsonar.host.url=http://localhost:9000 ^
                    -Dsonar.login=${env.SONAR_TOKEN} ^
                    -Dsonar.junit.reportPaths=target/surefire-reports ^
                    -Dsonar.java.binaries=target/classes
                """
            }
        }

        stage('Archive Reports') {
            steps {
                echo 'Archiving TestNG reports...'
                archiveArtifacts artifacts: 'test-output/**/*.html', fingerprint: true
                archiveArtifacts artifacts: 'test-output/**/*.xml', fingerprint: true
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging project...'
                bat 'mvn clean package'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build, Test & SonarQube Analysis completed successfully!'
        }
        failure {
            echo 'Build failed! Check logs for details.'
        }
    }
}
