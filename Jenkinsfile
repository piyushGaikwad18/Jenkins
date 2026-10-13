pipeline {
    agent any
Jenkinsfile at the root level of your repository and commit the following code:
    tools {
        maven 'Maven-3.9'
        jdk 'JDK-17'
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
                sh 'mvn clean compile'
            }
        }
        stage('3. Unit Test') {
            steps {
                echo 'Running unit tests...'
                sh 'mvn test'
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
                sh 'mvn package -DskipTests'
            }
        }
        stage('5. Deploy') {
            steps {
                echo 'Deploying application artifact...'
                sh '''
                    echo "Deploying target file to destination server..."
                    cp target/*.jar /tmp/deployed-app.jar || true
                    echo "Deployment completed successfully."
                '''
            }
        }
    }

   post {
        success { echo 'Pipeline completed successfully!' }
        failure { echo 'Pipeline failed. Check the logs for details.' }
    }
}
