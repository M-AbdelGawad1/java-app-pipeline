pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE', defaultValue: 'my-java-app', description: 'Docker Image Name')
        string(name: 'DOCKER_TAG', defaultValue: 'latest', description: 'Docker Tag')
    }

    stages {
        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${params.DOCKER_IMAGE}:${params.DOCKER_TAG} .
                """
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${params.DOCKER_IMAGE}:${params.DOCKER_TAG}"
            }
        }

        stage('Parallel Tasks') {
            parallel {
                stage('Run Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }
                stage('Code Quality') {
                    steps {
                        echo 'Run SonarQube or Linter here'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
    }
}
