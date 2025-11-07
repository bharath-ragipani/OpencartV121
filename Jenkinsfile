pipeline {
    agent any

    environment {
        // SonarQube token stored as an environment variable
        SONAR_TOKEN = 'sqp_1991cf1af85d111cd8cce4d8ddbd21f4053fbdb8'
        MAVEN_HOME = 'C:\\path\\to\\maven' // Optional if Maven is in PATH
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo "Checking out source code..."
                git url: 'https://github.com/bharath-ragipani/OpencartV121.git', branch: 'master'
            }
        }

        stage('Build & Test') {
            steps {
                echo "Running Maven clean and test..."
                bat "mvn clean test"
            }
        }

        stage('Archive Reports') {
            steps {
                echo "Archiving TestNG and Extent reports..."
                archiveArtifacts artifacts: 'test-output/**/*, reports/**/*', allowEmptyArchive: true
            }
        }

        stage('Package') {
            steps {
                echo "Packaging project..."
                bat "mvn clean package"
                archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
            }
        }

       stage('SonarQube Analysis') {
    steps {
        echo 'Running SonarQube analysis...'
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
            bat """
            mvn sonar:sonar ^
            -Dsonar.projectKey=open-cart-app ^
            -Dsonar.projectName="open-cart app" ^
            -Dsonar.host.url=%SONAR_HOST_URL% ^
            -Dsonar.login=%SONAR_TOKEN% ^
            -Dsonar.sources=src/test/java
            """
        }
    }
}


    post {
        success {
            echo "Build & Test completed successfully!"
        }
        failure {
            echo "Build failed! Check logs for details."
        }
    }
}
