pipeline {
    agent any

    environment {
        // Define environment variables here
        MAVEN_HOME = tool 'Maven 3.6.3'
        JAVA_HOME = tool 'JDK 11'
        SONARQUBE_SERVER = 'SonarQubeServer'
    }

    stages {
        stage('Pre Build') {
            steps {
                echo 'Running Pre Build Steps...'
                // Add pre-build steps here
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh "${MAVEN_HOME}/bin/mvn clean compile"
            }
        }

        stage('Unit Test') {
            steps {
                echo 'Running Unit Tests...'
                sh "${MAVEN_HOME}/bin/mvn test"
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                // SonarQube environment variables
                SCANNER_HOME = tool 'SonarQube Scanner 4.6'
            }
            steps {
                echo 'Running SonarQube Analysis...'
                withSonarQubeEnv(SONARQUBE_SERVER) {
                    sh "${SCANNER_HOME}/bin/sonar-scanner -Dsonar.projectKey=your-project-key -Dsonar.sources=src -Dsonar.java.binaries=target/classes"
                }
            }
        }
    }

    post {
        success {
            echo 'Build succeeded!'
            mail to: 'supriyaveeramally@gmail.com',
                 subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Good news! The job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' was successful.\nCheck the details at ${env.BUILD_URL}"
        }
        failure {
            echo 'Build failed!'
            mail to: 'supriyaveeramally@gmail.com',
                 subject: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                 body: "Unfortunately, the job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' failed.\nCheck the details at ${env.BUILD_URL}"
        }
    }
}
