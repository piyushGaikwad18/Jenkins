
pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3.9'
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                echo 'Fetching source code from GitHub...'
                checkout scm
            }
        }

        stage('2. Build') {
            steps {
                echo 'Compiling the Java application...'
                bat 'mvn clean compile'
            }
        }

        stage('3. Unit Test') {
            steps {
                echo 'Running unit tests...'
                bat 'mvn test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('4. Package') {
            steps {
                echo 'Packaging application into JAR/WAR...'
                bat 'mvn package -DskipTests'
            }
        }

        stage('5. Deploy') {
            steps {
                echo 'Deploying application artifact...'
                bat '''
                    if not exist target mkdir target
                    if exist target\\*.jar (
                        if not exist "%WORKSPACE%\\deployment" mkdir "%WORKSPACE%\\deployment"
                        copy /Y target\\*.jar "%WORKSPACE%\\deployment\\"
                    ) else (
                        echo No JAR file found to deploy.
                    )
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
