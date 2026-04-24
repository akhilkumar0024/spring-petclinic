pipeline {
    agent any
    tools {
        jdk 'JDK-17' 
        maven 'Maven-3.9'
    }
    stages {
        stage('Checkout-Repo') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/akhilkumar0024/spring-petclinic.git']])
            }
        }
        stage('Build and Unit Test') {
            steps {
                sh 'mvn clean verify'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv(installationName: 'SonarQubeServer', credentialsId: 'SONARQUBE-TOKEN') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
    post {
        always {
            echo "Pipeline has finished execution"
            emailext body: """
            Job: $JOB_NAME
            Build: $BUILD_NUMBER
            Status: ${currentBuild.currentResult}
            Duration: ${currentBuild.durationString}
            URL: ${currentBuild.absoluteUrl}
            """,
            subject: "Build ${currentBuild.currentResult} - $JOB_NAME",
            to: 'akhilkumar0024@gmail.com'
        }
        success {
            echo "Congratulations! The build passed"
        }
    }
}