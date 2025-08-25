pipeline {
    agent any

    tools {
        maven 'M3'       // اسم الـ Maven زي ما معرفه في Global Tool Configuration
        jdk 'JDK-24'     // غيره لو عندك اسم تاني للـ JDK
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/M-AbdelGawad1/java-app-pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
    }

    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
