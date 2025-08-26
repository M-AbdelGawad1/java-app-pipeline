@Library('jenkins-shared-lib') _

pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE', defaultValue: 'my-java-app', description: 'Docker Image Name')
        string(name: 'DOCKER_TAG', defaultValue: 'latest', description: 'Docker Tag')
    }

    tools {
        maven 'M3'
        jdk 'JDK-24'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/M-AbdelGawad1/java-app-pipeline.git'
            }
        }

        stage('Build & Test') {
            parallel {
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
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    dockerBuildAndPush(params.DOCKER_IMAGE, params.DOCKER_TAG)
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}