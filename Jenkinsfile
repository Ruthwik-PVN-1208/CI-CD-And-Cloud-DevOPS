pipeline {
    agent any

    tools {
        jdk 'Java21'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target\\*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                bat '''
                for /f "tokens=2" %%a in ('tasklist ^| findstr java.exe') do taskkill /PID %%a /F
                start /B java -jar target\\*.jar > app.log 2>&1
                '''
            }
        }
    }
}
